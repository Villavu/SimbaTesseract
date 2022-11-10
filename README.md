## Simba Tesseract

Tesseract is an open source text recognition (OCR) Engine.
https://github.com/tesseract-ocr/tesseract

Tesseract attempts to read any text providing the image is processed enough.
This include will process the image with resizing and thresholding. 

Or you can pass a TMufasaBitmap image to be recognized.

----
### Why Resize?

Tesseract wants at least ~25 pixels height text. We can achieve this by scaling up the area.

----
### Why Threshold?

If a pixel is brightness is greater than a "threshold" value the pixel is marked as white else black.
This creates a binary image, which is an image that only contains two colors.

A picture is worth a thousand words...

Before:

![-0.1 threshold](images/before.png)

With `-0.1` threshold:

![-0.1 threshold](images/threshold01.png)

With `-0.9` threshold:

![-0.9 threshold](images/threshold09.png)

Much better!

----

Now if the text is **darker** than the background a **postive** threshold can be used:

Before:

![dark threshold](images/dark_before.png)

With `0.3` threshold:

![dark threshold](images/dark_threshold.png)

----

### Basic example:

This includes provides a `TesseractOCR` variable to use.

```pascal
{$I SimbaTesseract/Tesseract.simba}

begin
  // Return the text in area 100,100,200,200 with 0.9 threshold
  WriteLn TesseractOCR.Recognize(TBox.Create(100,100,200,200), -0.9);

  // Debug what was recognized above
  TesseractOCR.Debug();
end.
```

----

### Available methods:

```pascal
// Only match these characters
procedure TTesseractOCR.SetWhitelist(Value: String); 

// Do not match these characters
procedure TTesseractOCR.SetBlacklist(Value: String);

// Clear whitelist characters
procedure TTesseractOCR.ClearWhitelist;

// Clear blacklist characters
procedure TTesseractOCR.ClearBlacklist; 

// Drop characters with a match less than `Value`
procedure TTesseractOCR.SetMinMatch(Value: Single);

// OCR with resizing and threshold
function TTesseractOCR.Recognize(Area: TBox; Threshold: Single): String; overload;

// OCR on a bitmap that has been processed by yourself
function TTesseractOCR.Recognize(Bitmap: TMufasaBitmap): String; overload;

// Return the previous recognize bounds
function TTesseractOCR.GetTextBounds: TBox;

// Debug the previous recognize
procedure TTesseractOCR.Debug;

// Return all character matches of the previous recognize
function TTesseractOCR.GetCharacterMatches: TTesseractMatchArray;

// Return all word matches of the previous recognize
function TTesseractOCR.GetWordMatches: TTesseractMatchArray;

// Return all line matches of the previous recognize
function TTesseractOCR.GetLineMatches: TTesseractMatchArray;

// Return all block (paragraph) matches of the previous recognize
function TTesseractOCR.GetBlockMatches: TTesseractMatchArray; 
```