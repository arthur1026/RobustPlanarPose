Pose Estimation For Planar Target
by Nghia Ho

http://nghiaho.com/?page_id=576

This is an OpenCV port of Robust Pose Estimation from a Planar Target (2006) by Gerald Schweighofer and Axel Pinz using their Matlab code from the link in the paper. It is used to determine the pose of a planar target. It can be used for Augmented Reality to track a planar target, such as a business card.

The algorithm is very robust and does not require accurate calibration of the camera for Augmented Reality applications. I found that a rough guess for the FOV of my webcam worked.

Changes from last version

Added a check to see if the root finding code returned any valid solutions (thanks to csantos)
Requirements

OpenCV 2.x or later

Usage

On Linux, type make to compile the demo code and ./demo to see how it works. On Windows you have to make your own project.

Note: For Visual Studio users you might get a compiler error about undefined M_PI_2, please visit http://msdn.microsoft.com/en-us/library/4hwaceh6.aspx for a fix.

To use this code in your own program you only need to call the function

RPP:Rpp(const cv::Mat &model_3D, const cv::Mat &iprts, cv::Mat &Rlu, cv::Mat &tlu, int &it1, double &obj_err1, double &img_err1);

A description of the parameters are as follows:

model_3D are the fixed 3D points of the object you want to track, they should never change throughout the program. For example, say we have a square card we want to track, we can arbitrarily choose (-1,-1,0), (-1,1,0), (1,1,0), (1,-1,0) as the 4 corners point in 3D space. The values you choose will determine the reference frame for which the estimated pose will be relative to. Using the same example, (0,0,0) in this case will be at the centre of the square card.

iprts are the normalised image points. You will need to form a typical 3×3 camera matrix to normalise the points, which we shall call K. K typically looks like

    [ fx 0  cx ]
K = [ 0  fy cy ]
    [ 0  0  1  ]

fx/fy are the focal lengths (in pixels) and cx/cy are the optical centres. For Augmented Reality application, I find you can get away with rough approximations. For example, say the webcam has a horizontal FOV of about 60 degrees and the image size is 640×480. Then the 4 parameters are:

cx = 640/2 = 320
cy = 480/2 = 240
fx = fy = cx/tan(60/2 * pi / 180) = 554.26

Now that you have K, to normalise the points simply do

[ x' ]         [ x ]
[ y' ] = K^(-1)[ y ]
[ 1  ]         [ 1 ]

where x and y are the un-normalised image points

Rlu is the returned 3×3 rotation matrix. If you pass in a valid 3×3 matrix the it will be used as the initial guess of the rotation. In a tracking scenario you would continuously feedback the returned Rlu as input for the next frame.

tlu is the returned 3×1 translation vector

it1 is the number of iterations done

obj_err1 is the object projection error in 3D

img_err1 is the image projection error, in normalised image point space

License

Simplified BSD License, unless superseded by a license from the original Matlab code or the copy and paste code for Rpoly.cpp.
