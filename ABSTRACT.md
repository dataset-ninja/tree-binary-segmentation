**Tree Binary Segmentation** dataset is the result of the Biome App by Earthshot Labs. Biome is an iOS application for measuring trees in the field and creating forest inventories– an essential component of carbon accounting. Smartphones are well suited for capturing tree statistics such as diameter, height, and species, along with geolocation. In addition to recording these measurements, Biome organizes the data in a convenient format for researchers and technicians. While still under development, Biome represents a significant advancement in forest inventory practices, which currently require manual measurements using measuring and flagging tapes, stakes, clinometers, and notebooks. By digitizing this process, Biome can standardize and scale the inventorying of forests. Traditional forest inventories can be prone to user error, data loss, and the methods may vary in different regions of the world. Creating a mobile application solution that tackles these challenges will not only allow for larger, more standardized datasets, but it will also allow untrained users to contribute to forest restoration projects.

In a typical forest inventory, a field worker uses a measuring tape to measure the diameter of a tree at a height of 1.3 meters. Scientists refer to this as "diameter at breast height” or “DBH” as it represents a diameter measurement of the tree trunk taken at the height of an average human’s chest. With the Biome app, capturing a DBH measurement is as easy as taking a picture of a tree. This represents a significant improvement over traditional methods. A measuring tape is cumbersome and may require two people for large trees. The surface of trees may have spikes and some are home to insects such as venomous ants, which means that not needing to touch them or get close to them is a significant advantage. Biome’s biggest advancement is probably its speed. During authors recent field trip to Panama, their team practiced measuring sample plots of 5 and 10 meters in diameter. Authors split into two teams. The first was a team of three using traditional methods and the second was a single person using the application. From this simple experiment, it quickly became clear that the Biome app was approximately five times faster than three people taking manual measurements for the same plot.

![fig1](https://i.ibb.co/H7VfssH/63ca109aa8dd6ba1c8b671ba-Measuring-Thorny-Palm-p-800.webp)

<i>This palm tree with large spikes is extremely difficult to measure with DBH tape.</i>

![fig2](https://i.ibb.co/f9JpnVK/63ca10866ca26489f6504630-Biome-Thorny-Palm-p-1080.webp)

<i>Measuring this spikey palm tree with Biome is easy. The segmentation does a good job even with the irregular trunk.</i>

Authors have two machine learning models that make this possible: a binary semantic segmentation model and a regression model.

A binary semantic segmentation model is used to localize the trunk at a pixel level on the camera image, allowing us to determine the width of the trunk. Authors segmentation model is robust enough to recognize all types of trees, even oddly shaped ones that are common in the dense jungle environments where carbon projects take place. Many trees have bent trunks, spiky trunks, and vines growing around them, so authors developed their AI model for a diverse selection of trees. 
‍
In addition, authors model was specifically trained to avoid “false positives”. When the user takes a picture of a post, a bottle, or a lamp, it won’t be detected as a tree. The model also focuses on a single tree in the foreground. This is important for the measurement process which can be hindered by multiple trees being grouped together. 

![fig3](https://i.ibb.co/pKMTZsQ/63ca1a9731df2fbdcded71a4-Biome-segmentation-wip-p-800.webp)

<i>Authors training set includes "false positives", like this pole on a sidewalk.</i>

![fig4](https://i.ibb.co/rvjxKRH/63ca107682c5ea808794a18c-Biome-Activeloop-p-800.webp)

<i>Authors used Scale.ai for batch labeling, and then augmented the dataset for a result of 3030 pairs (img+mask). They store the datasets on [Activeloop](https://www.activeloop.ai/).</i>

Biome also uses a regression model trained to correlate the diameter of a tree with the pixel width of the segmentation mask as well as the distance of the phone’s camera to the tree trunk. Authors are able to calculate the distance easily using the Lidar sensor on the most recent iPhones. The average error of their model in their test set was 1.66cm. Their test set was created by measuring several trees’ DBH with a DBH tape. Authors made sure to have a wide range of diameters (5-140cm) in the set in order to test a range of widths.

![fig5](https://i.ibb.co/4SxsnGt/63cb0f2c83538417294c0ffe-regression-graph-DBH-p-800.png)

This graph shows the differences between Biome calculated DBH values versus manual measurements taken of the same trees. The goal here is to have the DBH measured with Biome as close as possible to the DBH measured by hand (green line). The closer the data points are to this line, the smaller their error is over traditional methods. In this case, the measurements with the Biome app match the DBH measured by hand reasonably well, except for the highest data point, which was measured manually at 140 cm while the Biome app estimated the diameter at ~118 cm. 

The error for very small and very large trees represents the operating range authors initially assumed when gathering data. To improve the model, they plan to gather data outside the 10-100 cm range. Of course, there are many useful applications for measuring trees outside of this range, for example: measuring small saplings in the first few years of growth.

<i> Please note the discrepancy in the number of images in the example(3030) and in the dataset(2718).</i> 
