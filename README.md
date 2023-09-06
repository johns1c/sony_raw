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

* wikipedia is a good start
* adobe tiff specs are on the net in PDF and RFC format
* japanese specs are readable online but cost to download
* have not yet found anything direct from SONY
* microsoft raw codec handles my files (windows  11)
* suspect libraw can also help
* EXIF tools is useful and can dump some of its reference tables

## TIFF like files
```
tba
    TIFF/EP  ISO 12234-2

    TIFF/FX   

IPD based file systems
    TIFF 6 (baseline not JPEG as per Adobe spec)
        TIFF 6 (baseline + new JPEG)
        TIFF 6 with extensions (but which follow general rules below)
            TIFF-F (Tiff 6 profile for saving faxes to file RFC2306 / Content-type: image/tiff; application=faxbw
)

    TIFF/IT (ISO 12639 obsolete? standard for the exchange of digital adverts and complete pages)
        TIFF/IT profile 1
        TIFF/IT profile 2
        
    EXIF based
        GUidelines for DSC (based on EXIF and TIFF6?) 
            Manufactures file type meeting guidelines e.g. ARW
                 Particular variant for example with specific camera software or image sensor or way of combining image
                 
```       


### Standard TIFF (Baseline Tiff 6 plus JPEG revisions)

#### General Rules
* if multiple  IFDs the first one must be the full size image (Tiff 6)
* all offsets must point to real things and not be zero
* No duplicate pointers (guess this makes it editable)
* the standard formats have BASELINE TAGS which must be present in the  first IFD
* can register tags but if you want a lot use one to point to own IFD wth private tags
* there is a private tag range
#### Baseline Tags

Baseline TIFF per [TIFF] requires that the following fields be present for all BiLevel Images:  
**ImageWidth, ImageLength,
Compression, PhotometricInterpretation, StripOffsets, RowsPerStrip,
StripByteCounts, XResolution, YResolution and ResolutionUnit**.  

if fields are omitted, the Baseline TIFF default value(if specified)
will apply.

## TIFF-F Rules

uses all baseline fields but range of accptable values may be different

* Uncompressed data is *not* allowed
* IPDs in page order
* data *should* follow IPD and preceed next page (think that this just makes it eaier to write the file page by page)
* data *should* have one strip per page
* compression 3 and 4 only
* T4Options or T6Options tags as required by type of compression= 3 or 4
* NewSubFileType bits set for single page of multi-page image (even for one page documents)
* should have page numbers x of y (x of zero if total pages unknown)
* additional 3 data quality tags CleanFaxData,  BadFaxLines, and ConsecutiveBadFaxLines fields:
1.  Facsimile hardware does not provide page quality information: MUST NOT write page-quality fields.
2.  Facsimile hardware provides page quality information, but reports no bad lines.  Write only BadFaxLines = 0.
3.  Facsimile hardware provides page quality information, and reports bad lines.  Write both BadFaxLines and ConsecutiveBadFaxLines.  Also write CleanFaxData = 1 or 2 if            the hardware's regeneration capability is known.
4.  Source image data stream is error-corrected or otherwise guaranteed to be error-free such as for a computer generated file:  SHOULD NOT write page-quality fields.
  
