% intensity transformaion in to negative image
f = imread('Picture1.png'); 
g = imcomplement(f);
imshow(f);
figure;
imshow(g);



