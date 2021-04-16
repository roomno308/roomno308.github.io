---
layout: post
title: Image of Vanishing Point and Rotation Matrix
author: 308
---

We know that North Star was used by sailors to navigate through the seas. North Star can be thought of as a Vanishing Point in "North" Direction and the task of navigation using Polaris is nothing but the task of determining the rotation of a ship (navigation part comes later when, with the help of a map, you would know where to rotate the ship). So, this (very interesting) idea can be used in Computer Vision to find a camera's rotation matrix from the image (projection) of vanishing points.

We know that the camera projection equation looks like:
\\[p = K[R \; t]X\\]

Where \\(p\\) is the point in image co-ordingates \\([u, v,1]^T\\), \\(K\\) is the camera caliberation matrix, \\(R\\) and \\(T\\) are the rotation and translation respectively, and \\(X\\) is the poin in world co-ordinate \\([x,y,z,d]^T\\) (in homogeneous coordenates) of which image is taken.

Now, if we put a vanishing point (say, vanishing point corresponding to \\(\hat{x}\\)) in place of \\(X\\) in the camera projection equation:

\\[ v_\hat{x} = K[R \; t]\\]

where \\(v_\hat{x}\\) is the image of the vanishing point.

\\[\implies v_\hat{x} = K.R_{col(1)}\\]

Where \\(R_{col(1)}\\) is the 1st column of \\(R\\). In general,

\\[\implies v_i = K.R_{col(i)}\\]