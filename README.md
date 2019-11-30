PCL概览：
Filters：滤波器：
pcl使用的滤波方法：计算每个点到周围所有点的距离，求出距离的平均值。同时，在所有点的范围中，计算出距离平均值的均值和标准差。假设所有距离平均值符合高斯分布，那么通过均值和标准差可以算出距离值的范围，偏离这一范围的点将被去除。

Features:特征：
利用数据结构和机制，从点云数据估算3d特征。比如：底层表面的曲率估计和观测点p附近的曲面法线。做法通常是，一，使用空间分解技术,如八叉树或kD-trees分成小块。二，取点。方法有：附近距离最近的k个点，或者周围距离小于k的球体中的所有点。三：特征分解（计算k个临近点所构成平面的特征值和特征向量）。最小的特征值所对应的特征向量，近似为点p的曲面法线n,而表面曲率变化可以根据特征值估计出来。l0/(l0+l1+l2) l0<l1<l2,l是lamda，特征值。

Keypoints：关键点：
关键点是图像或点云中稳定的,独特的,可以用特定的检测标准检测出的点。关键点和特征描述，二者可以概括性地描述点云的特征。
form a compact—yet descriptive—representation of the original data.

Registration: 注册：
关键是确定数据集里面的对应点，找到一个转换，可以最小化对应点之间的距离(对齐错误)。

Kd-tree：K维树
k维树是一个用于分割空间的树形的数据结构，适用于高效的范围搜索和最近邻搜索。最近邻搜索是处理点云数据的核心操作，可以用来找到对应点，或特征描述符，或定义一个点或点周围的附近区域。

Octree: 八叉树
八叉树库提供了有效的方法用于创建从点云数据生成的分层数据结构。在操作点处进行空间分区,降采样和搜索。每个八叉树节点都有八个孩子或者没有孩子。根节点描述了一个封装所有点的立方边界框。在每棵树的层面上,这个空间被细分2倍，以增加分辨率。
未看完

Segmentation：切割：
细分库把点云分割到不同的集群。这些算法最适合处理由空间隔离的点云。在这种情况下,集群通常用于把点云分解成它的多种组成部分,然后可以独立处理。

Sample Consensus:一致性算法：
一些模型中，实现这个库包括:线、飞机、圆柱体和球体。平面拟合通常应用于检测常见的室内表面的任务,如墙壁、地板和桌面。其他模型可以用来检测和分割常见的几何结构(例如,用圆柱体拟合杯子)。

Surface：表面：
例如船体,网格表示或平滑/用法线重新取样。

网格是一个通用的方法，来创建一个表面的点,目前有两种算法提供:非常快的三角化拆分,有较慢的啮合、平滑和填洞。

创建一个凸或凹船体是有用的,例如当需要一个简化的表面表征或当需要提取边界时。

Range Image：深度图
深度图是的像素值代表距离或到传感器的光心的深度。范围是一个常见的3D图像,通常由立体相机或结构光相机。了解相机的内在的校准参数,一系列图像可以被转换成一个点云。

I/O：
阅读和写作点云数据(PCD)文件,以及从各种传感设备获取点云。

Visualization：可视化：
可视化库的目的是能够快速构建原型，可视化算法的结果。

Common：常用库：
常见的库包含常见的数据结构和方法。核心数据结构包括PointCloud类和众多的点类型,用于表示点,表面法线,RGB颜色值,特征描述符,等。它还包含许多功能计算距离/规范,均值和方差,角转换,几何变换等等。

Search：搜索：
搜索库提供的方法寻找最近邻,包括:K维树，八叉树，蛮力，专门的搜索算法。

Binaries：文件：
pcl_viewer: 

pcl_viewer <file_name 1..N>.<pcd or vtk> <options>, where options are:
-bc r,g,b = background color
-fc r,g,b = foreground color
-ps X = point size (1..64)
-opaque X = rendered point cloud opacity (0..1)
-ax n = enable on-screen display of XYZ axes and scale them to n
-ax_pos X,Y,Z = if axes are enabled, set their X,Y,Z position in space (default 0,0,0)
-cam (*) = use given camera settings as initial view
(*) [Clipping Range / Focal Point / Position / ViewUp / Distance / Field of View Y / Window Size / Window Pos] or use a <filename.cam> that contains the same information.
-multiview 0/1 = enable/disable auto-multi viewport rendering (default disabled)
-normals 0/X = disable/enable the display of every Xth point’s surface normal as lines (default disabled) -normals_scale X = resize the normal unit vector size to X (default 0.02)
-pc 0/X = disable/enable the display of every Xth point’s principal curvatures as lines (default disabled) -pc_scale X = resize the principal curvatures vectors size to X (default 0.02)
(Note: for multiple .pcd files, provide multiple -{fc,ps,opaque} parameters; they will be automatically assigned to the right file)

Usage example:
pcl_viewer -multiview 1 data/partial_cup_model.pcd data/partial_cup_model.pcd data/partial_cup_model.pcd
The above will load the partial_cup_model.pcd file 3 times, and will create a multi-viewport rendering (-multiview 1).
_images/ex11.jpg
pcd_convert_NaN_nan: converts “NaN” values to “nan” values. (Note: Starting with PCL version 1.0.1 the string representation for NaN is “nan”.)

Usage example:
pcd_convert_NaN_nan input.pcd output.pcd
convert_pcd_ascii_binary: converts PCD (Point Cloud Data) files from ASCII to binary and viceversa.

Usage example:
convert_pcd_ascii_binary <file_in.pcd> <file_out.pcd> 0/1/2 (ascii/binary/binary_compressed) [precision (ASCII)]
concatenate_points_pcd: concatenates the points of two or more PCD (Point Cloud Data) files into a single PCD file.

Usage example:
concatenate_points_pcd <filename 1..N.pcd>
(Note: the resulting PCD file will be ``output.pcd``)
pcd2vtk: converts PCD (Point Cloud Data) files to the VTK format.

Usage example:
pcd2vtk input.pcd output.vtk
pcd2ply: converts PCD (Point Cloud Data) files to the PLY format.

Usage example:
pcd2ply input.pcd output.ply

mesh2pcd: convert a CAD model to a PCD (Point Cloud Data) file, using ray tracing operations.
Syntax is: mesh2pcd input.{ply,obj} output.pcd <options>, where options are:
-level X = tessellated sphere level (default: 2)
-resolution X = the sphere resolution in angle increments (default: 100 deg)
-leaf_size X = the XYZ leaf size for the VoxelGrid – for data reduction (default: 0.010000 m)

octree_viewer: allows the visualization of octrees
Syntax is: octree_viewer <file_name.pcd> <octree resolution>

Usage example:
Example: ./octree_viewer ../../test/bunny.pcd 0.02
