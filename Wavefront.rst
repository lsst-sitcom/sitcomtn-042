Inspired by the derivations outlined in :cite:p:`2015ApOpt..54.9045X`, we have developed a new method for estimating uncorrected wavefront errors in the images of Auxtel. 
To do this requires two sets of donuts, a set taken intra-focal and a set taken extra-focal. To test our mehtod we used data from the October 2022 Auxtel run specifically we used images ``seq_num`` 578-588. 
These images were taken for this analysis especially, they were made with intentionally displacing focus 7.5mm on either side of focus. 

background
==========
Starting from with the derivation in :cite:p:`2015ApOpt..54.9045X`. Let us start by defining the wavefront signal :math:`S`:

.. math::
    S = -\frac{1}{I_0}\frac{\partial I}{\partial z} \approx -\frac{1}{\Delta z}\frac{I_1 -I_2}{I_1 + I_2}.

Where the focus offset :math:`\Delta z` in object space is given by:

.. math:: \Delta z = f(f-\ell)/\ell 

Where :math:`f` is the focal length and :math:`\ell` is the defocus distance, that is how far from focus the two images :math:`I_1` and :math:`I_2` are taken from focus. 
One image taken intra-focal and the other extra-focal.

It turns out we can approximate the Laplacian of the wavefront error as:

.. math:: \nabla^2 W \approx S

From this we get the idea that we can get the tilt of the wavefront as the integral of the S with respect to :math:`x` and :math:`y` in the pupil frame:

.. math::
    T_x(x,y) = \int_{-R_x}^x S(x',y) dx'\\
    T_y(x,y) = \int_{-R_y}^y S(x,y') dy' 

Where :math:`R_{x/y}` is the radius of the donut, with our cordinate system centered on the donut center. 
This corresponds to taking the cummulative sum in the :math:`x` and :math:`y` directions. 
Adding these in Quadrature we obtain an estimate for the Pixel tilt:

.. math:: 
    T_\text{pixel} = \sqrt{T_x^2+T_y^2}

Finally we can from the Pixel tilt obtain the RMS of the this tilt, which should correspond to the RMS of the residual wavefront error. 

.. Warning:: We are currently having an issue where there seems to be a factor 2 difference between the RMS from tilt and the theoretical RMS for the wavefront error.


Data processing:
================

We processed the donuts by first isolating the each of the 11 donuts. To do this we find the center of each donut utilizing the same methods as utilized in :cite:p:'sitcom-27'. Once each donut has been isolated and we found centers for them, we average over the 5 donuts we have at each position.
We make cut-outs around each donut, utilizing the center of the donut and the finding the largest radius of the donuts. Next we take the averages of the images on either side of focus, followed by a masking of the donuts.
We utilized two different masking techniques, first we made circular masks using the information obtained about the circles of the donut to generate and artificial donut mask that fit with the average donut image. 
The second method we instead utilize the masking used in :cite:p:'sitcom-27' to generate the masks. The advantage of the second method is that we get the spiders in the donuts cut out. 
After having masked the averages on either side of the focus, we the process them as indicated in the background section.


Simulations:
============

As a sanity check of the method we conducted our analysis on both real data and on a set of simulated donuts.
We generated the donuts using Josh Myers' Donut generating code `Danish <https://github.com/jmeyers314/danish>`_, which is based on his batoid code `Batoid <https://github.com/jmeyers314/batoid>`_.

We generated donuts with a set of configurations that should correspond to the Auxtel setup. We analysed a set that was displaced :math:`0.75mm` from focus. 


Results:
========

From the data processing we obtain the follwing results, dependent on our masking.

.. figure:: /_static/result_20221012_7.5_mask1.png
    :name: simple_mask_res 

    We see here the results utilizing the simple circular mask to include all of the donut, but not cutting out the spiders. 

.. figure:: /_static/result_20221012_7.5_mask2.png
    :name: Results_with_advanced_mask

    Here we utilized the more advanced mask, it cuts out parts of the donut that fall below a certain cutting value, but it has the advantage that it also cuts out the spiders in the donuts

Once we have obtained the tilts we can then estimate the RMS of the pixel tilt:

We obtain for the simple mask: 0.077264264 

While for the advanced mask we obtain: 0.07585939

So we see very little difference. we also see that the uncorrected wavefront error is of the order :math:`10^{-2}` arcsecs.