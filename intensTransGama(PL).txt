% intensity Transformation Power Law (gamma)
f = imread('Picture1.png'); 
g = imadjust(f, [ ], [ ], 0.5);
imshow(f);
figure;
imshow(g);
