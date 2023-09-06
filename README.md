# sony_raw
My notes on tiff / raw files and samples

DSC01044.ARW was taken a SONY RX100-M2 The sample DSC-RX100M2 v1.10
it was taken  by myself and I am happy for it to  be in the Public Domain

it is of Offas Dyke where it crosses the Kerry Ridgeway
for other samples and notes- see alevey/sony_raw

# Notes

## Sony ARW files are TIFF like

The first 16 bytes match that of a standard TIFF and the IFDs behave like normal with my example having
two IFDs at the top  level. 

Sony has a second level of IFDs including thier own and EXIF ones.  They use additional tags and some tags which in a TIFF are at the  top level are at lower levels in the ARW (think that this corresponds to Japanese standards).  Exif tool documentation gives the meaning of the  tags

In particular the first IFD does not contain some tags such as ImageSize which are required by standard TIFF (though are these mandatory onlly for their given compression types).  This mans that the file cannot be opened by Python Pillow but gets past accept function.

The file contains three images (think that all are in sub-files) 
* the main one 
* a preview 
* a thumbnail

## Reference

* wikapedia is a good start
* adobe tiff specs are on the net in PDF and RFC format
* japanese specs are readable online but cost to download
* have not yet found anything direct from SONY
* microsoft raw codec handles my files (windows  11)
* suspect libraw can also
  

  









S
Sony uses additional tags and what I call sub IFDs 

