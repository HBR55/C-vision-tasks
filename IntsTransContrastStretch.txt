% Intensity Transformation Contrast Stretching 
f = imread('seeds.png'); 
fd = im2double(f); 
m = mean2(fd) 
g = 1./(1+(m./(fd+eps)).^4); 
imshow(f); 
figure;
imshow(g);
