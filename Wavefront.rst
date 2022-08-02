Inspired by the derivations outlined in :cite:p:`2015ApOpt..54.9045X`, we have developed a new method for estimating uncorrected wavefront errors in the images of Auxtel. 
To do this requires two sets of donuts, a set taken intra-focal and a set taken extra-focal. To test our mehtod we used data from the March 2022 run specifically we used images ``seq_num`` INSERT SEQ. 


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

We processed the donuts by first isolating the each of the 20 donuts we on each side of focus. To do this we find the center of each donut utilizing the code module written for :cite:p:'sitcom-27'. Once each donut has been isolated and we found centers for them, we average over the 5 donuts we have at each position.
Next we match up the 2 donuts we have on either side of the focus. From the information obtained on the inner and outer radius of the now 10 initial donuts, we generate a common mask for the 2 donuts on either side of the focus. 
After applying this mask we can then proceed to add the images to obtain the Wavefront signal as described above. 

Simulations:
============

As a sanity check of the method we conducted our analysis on both real data and on a set of simulated donuts.
We generated the donuts using Josh Myers' Donut generating code `Danish <https://github.com/jmeyers314/danish>`_, which is based on his batoid code `Batoid <https://github.com/jmeyers314/batoid>`_.

We generated donuts with a set of configurations that should correspond to the Auxtel setup. We analysed a set that was displaced :math:`0.8mm` from focus. 


Results:
========