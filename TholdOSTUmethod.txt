%Thresholding using Otsu Method: Example
I = imread('COINS.png');
imhist(I)
level = graythresh(I)
level = 0.4941
BW = im2bw(I,level);
imshow(BW)

