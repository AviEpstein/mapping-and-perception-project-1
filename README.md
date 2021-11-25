# mapping-and-perception-project-1-occupancy-map-and-ICP-algorithim

In this assignment we will be analyzing and creating an occupancy map using the kitty data set recording: 2011_09_26_drive_0095.

The data set contained:
256 frames of-
Lidar records compatible to each frame - velodyne points(m)
lat: latitude of the oxts-unit (deg) 
lon: longitude of the oxts-unit (deg) 
alt: altitude of the oxts-unit (m) 
roll: roll angle (rad), 0 = level, positive = left side up, range: -pi .. +pi 
pitch: pitch angle (rad), 0 = level, positive = front down, range: -pi/2 .. +pi/2 
yaw: heading (rad), 0 = east, positive = counter clockwise, range: -pi .. +pi 

the data can be dowloaded from: The KITTI vision benchmark suite

![image](https://user-images.githubusercontent.com/73026385/143426948-86477faa-5432-4550-8ef6-a84dfbfb0c8b.png)


We will be creating a probabilistic occupancy grid map using the inverse range sensor module checking different parameters for Hit/Miss prob and Threshold
We will then corrupt the imu data and attempt to correct our occupancy grid map using the icp algorithm.

The flow+main functions:




Question 2 making occupancy map - 
Make_occupancy_map -> looped: Birds_eye_point_cloud -> Find_messerment_from_lidar_to_vehichle - > occupancy_grid_map -> Inverse_range_sensor_model 
Functions:
Birds_eye_point_cloud - 
Takes a point cloud received by lidar, translates it to body coordinates, if needed discards points that are too far away and returns the point cloud, or transforms the point cloud to image coordinates making it a birds eye view.

Find_messerment_from_lidar_to_vehichle - Transformers an image coordinate birds eye and calculates z_t the range and the angel from the forward heeding -180/180

Inverse_range_sensor_model - returning logit probabilities if occupancy cell is hit free kisse or out of range based on matching body angles and radius to cell and z_t.

Occupancy_grid_mapping - driver of the inverse_sensor function on every cell

Make_occupancy_map - driver of occupancy_grid_map, Birds_eye_point_cloud  function,Find_messerment_from_lidar_to_vehichle, for each frame.




Question 3 using ICP algorithm to correct currupted imu data and recreating occupancy map - 

implementations  flow + main functions:
Perform_icp_between_clouds  - At each occupancy update 2 frames were grabbed first point cloud was translated and rotated by the difference in angle and distances,uniformly subsamples the indices then calls icp_svd after this process is finished point cloud 1 is rotated back to original location and the building of the map continues as before.

Icp_svd - performs icp using svd finds calls centers the data get_correspondence_indices by kd tree alg then finds cross covariance removes unwanted indices using kernel outlier rejection computes svd rotates first point cloud and then repeats proces a number of iterations.

Make_occupancy_map - runs occupancy map with icp correction needed witch then runs all frames throw Perform_icp_between_clouds 

Make_occupancy_map  - > for every frame: Perform_icp_between_clouds  - > runs icp_svd




full results in gif file and full maps in project report.



path the car drove visualized in google maps:

![image](https://user-images.githubusercontent.com/73026385/143428341-632f4e2a-40e8-469e-9353-8ca82f74b0f2.png)

the occupancy map created:

![image](https://user-images.githubusercontent.com/73026385/143427910-8f566d87-fbb1-42f6-b3c7-536d3a24ffae.png)

occupancy map animation: (press the image to preview)
![Threshold 0 8 HitMiss prob 0_7_0_4_](https://user-images.githubusercontent.com/73026385/143428967-1e5ef486-7a55-4351-98a3-941f61d72968.gif)


written by Avi Epstein.
