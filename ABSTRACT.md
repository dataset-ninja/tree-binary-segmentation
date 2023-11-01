The **Tree Binary Segmentation** dataset originates from the Biome App developed by Earthshot Labs. Biome is an iOS application designed for on-site tree measurements and the creation of forest inventories, which play a critical role in carbon accounting. The use of smartphones for collecting data on tree attributes such as diameter, height, species, and GPS coordinates is a well-suited approach. Beyond data collection, Biome offers a user-friendly data organization format for researchers and technicians.

While it is still in the developmental phase, Biome represents a significant leap forward in the field of forest inventory practices. Conventional methods rely on manual measurements involving tools like measuring tapes, stakes, clinometers, and notebooks. By transitioning to a digital approach, Biome has the potential to standardize and streamline the process of forest inventorying. Traditional methods are susceptible to human error, data loss, and variations in techniques across different regions. The development of a mobile application like Biome not only allows for larger and more standardized datasets but also enables untrained individuals to contribute to forest restoration projects.

In a typical forest inventory, field workers use measuring tape to determine a tree's diameter at a height of 1.3 meters, commonly referred to as "diameter at breast height" or "DBH." This measurement represents the diameter of a tree's trunk taken at the height of an average person's chest. With the Biome app, capturing the DBH measurement becomes as simple as photographing a tree, marking a significant improvement over traditional methods. A measuring tape can be unwieldy, especially for large trees, often requiring two people. Trees may have uneven surfaces, thorns, or even house insects, such as venomous ants, making the non-contact approach offered by the app a substantial advantage.

One of the most noteworthy benefits of the Biome app is its speed. During a recent field trip to Panama, the research team conducted measurements in sample plots ranging from 5 to 10 meters in diameter. The team was divided into two groups: one using traditional methods (a team of three), and the other using the Biome application (a single person). This simple experiment clearly demonstrated that the Biome app was approximately five times faster at measuring the same plot compared to the three-person team using manual methods.

![fig1](https://i.ibb.co/H7VfssH/63ca109aa8dd6ba1c8b671ba-Measuring-Thorny-Palm-p-800.webp)

<span style="font-size: smaller; font-style: italic;">This palm tree with large spikes is extremely difficult to measure with DBH tape.</span>

![fig2](https://i.ibb.co/f9JpnVK/63ca10866ca26489f6504630-Biome-Thorny-Palm-p-1080.webp)

<span style="font-size: smaller; font-style: italic;">Measuring this spikey palm tree with Biome is easy. The segmentation does a good job even with the irregular trunk.</span>

Authors have two machine learning models that make this possible: a binary semantic segmentation model and a regression model.

A binary semantic segmentation model is used to localize the trunk at a pixel level on the camera image, allowing us to determine the width of the trunk. The authors' segmentation model is robust enough to recognize all types of trees, even oddly shaped ones that are common in the dense jungle environments where carbon projects take place. Many trees have bent trunks, spiky trunks, and vines growing around them, so authors developed their AI model for a diverse selection of trees. 
‍
In addition, the authors' model was specifically trained to avoid “false positives”. When the user takes a picture of a post, a bottle, or a lamp, it won’t be detected as a tree. The model also focuses on a single tree in the foreground. This is important for the measurement process which can be hindered by multiple trees being grouped together. 

![fig3](https://i.ibb.co/pKMTZsQ/63ca1a9731df2fbdcded71a4-Biome-segmentation-wip-p-800.webp)

<span style="font-size: smaller; font-style: italic;"> Author training set includes "false positives", like this pole on a sidewalk.</span>

![fig4](https://i.ibb.co/rvjxKRH/63ca107682c5ea808794a18c-Biome-Activeloop-p-800.webp)

<span style="font-size: smaller; font-style: italic;">Authors used Scale.ai for batch labelling, and then augmented the dataset for a result of 3030 pairs (img+mask). They store the datasets on [Activeloop](https://www.activeloop.ai/).</span>

Biome also uses a regression model trained to correlate the diameter of a tree with the pixel width of the segmentation mask as well as the distance of the phone’s camera to the tree trunk. Authors are able to calculate the distance easily using the Lidar sensor on the most recent iPhones. The average error of their model in their test set was 1.66cm. Their test set was created by measuring several trees’ DBH with a DBH tape. The authors made sure to have a wide range of diameters (5-140cm) in the set in order to test a range of widths.

![fig5](https://i.ibb.co/4SxsnGt/63cb0f2c83538417294c0ffe-regression-graph-DBH-p-800.png)

This graph shows the differences between Biome calculated DBH values versus manual measurements taken of the same trees. The goal here is to have the DBH measured with Biome as close as possible to the DBH measured by hand (green line). The closer the data points are to this line, the smaller their error is over traditional methods. In this case, the measurements with the Biome app match the DBH measured by hand reasonably well, except for the highest data point, which was measured manually at 140 cm while the Biome app estimated the diameter at ~118 cm. 

The error for very small and very large trees represents the operating range authors initially assumed when gathering data. To improve the model, they plan to gather data outside the 10-100 cm range. Of course, there are many useful applications for measuring trees outside of this range, for example: measuring small saplings in the first few years of growth.

<i>Please note the discrepancy in the number of images in the example(3030) and in the dataset(2718).</i> 

