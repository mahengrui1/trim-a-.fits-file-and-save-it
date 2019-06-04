# trim-a-.fits-file-and-save-it
20190604
怎么把.fits图像文件的一部分挖出来保存成新文件？（fits 图像剪切）（仅支持矩形） 
1 可以用CCDLAB （https://www.ucalgary.ca/uvit/）。这里没有教程。
2 可以用python3

参考代码（不全）
（Cutout2D可以保存WCS信息但我不会）
---
How do I dig out a part of a .fits image file and save it as a new file? (fits image cut) (rectangle only)
1 CCDLAB (https://www.ucalgary.ca/uvit/) can be used. There are no tutorials here.
2 can use python3

Reference code (not complete)
(Cutout2D can save WCS information but Idk)

---

import numpy as np
from astropy.nddata.utils import Cutout2D
from astropy import units as u

>>> np.asarray(data>3000).nonzero()
(array([3145]), array([3169]))

>>> (np.asarray(data>3000)*np.asarray(data<4000)).nonzero()
(array([3145]), array([3169]))

>>> data[3145,3169]
3390.763839125362
>>> cutout1 = Cutout2D(data, (2287, 2798), 200)
>>> cutout1
<astropy.nddata.utils.Cutout2D object at 0x7fa2294336a0>

>>> cutout1.data
array([[0.        , 3.00999738, 0.        , ..., 1.05250955, 1.03334084,
        0.        ],
       [1.02529704, 0.        , 1.0450534 , ..., 0.        , 1.076158  ,
        0.        ],
       [1.00353564, 0.        , 0.        , ..., 0.99406832, 0.        ,
        0.        ],
       ...,
       [0.        , 1.04063021, 0.        , ..., 0.        , 1.02758246,
        0.        ],
       [0.98425233, 0.        , 0.        , ..., 0.        , 1.02701015,
        0.93842073],
       [0.        , 1.00709816, 0.        , ..., 0.        , 0.93842073,
        1.97301   ]])

>>> grey=fits.PrimaryHDU(cutout1.data)

>>> grey
<astropy.io.fits.hdu.image.PrimaryHDU object at 0x7fa241098550>
>>> cutout1
<astropy.nddata.utils.Cutout2D object at 0x7fa2294336a0>
>>> greyHDU=fits.HDUList([grey])
>>> greyHDU.writeto('try.fits')
