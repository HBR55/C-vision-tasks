% Intensity Transformation Logarithmic
f = imread('light.png');
g = im2uint8(mat2gray(log(1+double(f))));
imshow(f); 
figure;
imshow(g);
