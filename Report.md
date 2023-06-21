#### `investigation of (machine learning)ml, ai & (computer vision)cv approaches to measuring accuracy of finished frames compared to actual shop drawings.`
    
[//]: #  ("cSpell.language": "en-GB")
    
## 20/02/2023
> tasks  
1. investigate different cameras that could be used to acquire data

> details
1. here we are trying to balance cost, effectiveness, robustness and viability of different camera modules that could be used to acquire images for cv. 

> results
1. a camera without zoom would need at least 108mp to achieve the resolution needed to see edges clearly and to read barcodes. a camera with pan, tilt, rotate (ptz) & zoom but with lower resolutions could be more effective. a ptz camera has been ordered to test this approach.

## 21/02/2023
> tasks  
1. test various image segmentation approaches to see is we can extract a frame from an image, given a similar image without the frame

> details
1. in the first case images taken by a drown were aligned using the sift algorithm and then pixel subtraction was used to highlight changes.

> results
1. results were reasonable for the frame with the fluro green surface but not fit for purpose for actual frames. this was due to the inconsistent colouring of actual timber.    

!["fluro frame"](./_report/fluro%20frame%20segmentation.jpg)
    

## 22/02/2023
> tasks
1. `(continued)` test various image segmentation approaches to see is we can extract a frame from an image, given a similar image without the frame

> details
1. opencv’s `structural similarity` algorithm was used to find better matching with/without images. then `absdiff` was applied to extract differences. these differences were then highlighted using 
   1. abs diff
   2. amplification
   3. thresholding
   4. `(best)` removal of isolated differences and retaining the largest fully connected object


> results  
1. this approach resulted in b&w images that were better suited to the process we wish to automate. row details for images below. from left to right.
   1. this was the base image difference the following steps used to improve upon
   2. pixels were amplified
   3. a threshold was applied where pixel values outside this threshold were removed
   4. the image from step 3. was fed into this algorithm and isolated pixels were removed. the largest object with connected white pixels was assumed to be the object of interest and only this was kept.
   5. colour image without frame
   6. colour image with frame

![segmentation 1](./_report/result%20timber.png)    
![segmentation 2](./_report/230222_1634%20result%200%20-%204.png)    
![segmentation 3](./_report/230222_1634%20result%204%20-%208.png)    

## 23/02/2023
> tasks
1. setup remote development machine with dedicated gpu
2. investigate linear array of sensors to detect shape of frames as they pass by
3. tag pixels in images so they can be used for a cnn classification approach
   1. 0 for frame
   2. 1 for workshop
4. test accuracy of ml approach for timber detection

> details
1. gpu is needed to run ml tasks
2. can we scan a frame as it passes by and create a 3d model from the result
3. the idea here is to train a model to recognise timber. if successful it may help to separate the frame from the background workshop
   1. ideally, we will then break the image up into smaller sub images (255x255 pixels) and then using the pixel tags(above) then tag each of these sub images as 
      1. 0 - timber
      2. 1 - workshop
      3. 3 - interface of timber and workshop
4. try finding the range of rgb values for the frame and remove anything not in this range

> results
1. complete
2. nothing stood out, but we may revisit this with the use of sonar or radar. sonar may be more accurate for our needs as sound is slower and at shorter distances this may be more accurate. we could also use cameras in a linear array to film the frame as it passes by and then stich the images together. we may also need to mark the rollers with a matrix to determine the velocity of the frame as it passes by. this can be done by recording the spinning matrix.
3. images were broken into 20x20 sub images and average pixel values calculated. 255 - is workshop and 0 is frame. 127 should be 50/50
4. the background contained too many objects of similar hue
![hue extraction](_report/pixel%20hue%20extraction.png)
![rgb distribution](_report/rgb%20distrabution%20of%20frame.png)
### distribution of rgb pixels (frame - only)
<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>x</th>      <th>y</th>      <th>z</th>    </tr>  </thead>  <tbody>    <tr>      <th>count</th>      <td>83253.000000</td>      <td>83253.000000</td>      <td>83253.000000</td>    </tr>    <tr>      <th>mean</th>      <td>148.742242</td>      <td>168.740408</td>      <td>156.722849</td>    </tr>    <tr>      <th>std</th>      <td>27.947245</td>      <td>27.092532</td>      <td>41.465251</td>    </tr>    <tr>      <th>min</th>      <td>32.967500</td>      <td>41.382500</td>      <td>39.672500</td>    </tr>    <tr>      <th>25%</th>      <td>130.695000</td>      <td>152.320000</td>      <td>131.967500</td>    </tr>    <tr>      <th>50%</th>      <td>147.525000</td>      <td>163.707500</td>      <td>152.122500</td>    </tr>    <tr>      <th>75%</th>      <td>164.425000</td>      <td>182.382500</td>      <td>182.035000</td>    </tr>    <tr>      <th>max</th>      <td>214.492500</td>      <td>229.922500</td>      <td>252.947500</td>    </tr>  </tbody></table>

## 24/02/2023
> tasks  
1. create a pipeline to process images. these images will then be used as an input to a transfer learning model.
2. divide the overhead image into smaller patches (~50x50 pixels) and tag each one as workshop or timber

> details
1. using a windows paint utility, frames were extracted from the shop background and the background was set to white. this was then used to tag areas of the workshop that are workshop and areas that are frames. 
2. 

> results
1. tagging was successful, but the pipeline required a lot of manual manipulation. on monday the pipeline will be automated as much as possible


