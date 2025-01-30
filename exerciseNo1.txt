%exercise code
%Identify which intensity transformation was used on liftingbody.png to create each of the four results below. 
%Write a script to reproduce the results using the intensity transformation functions.
%--------------------------------------------------------------------------------------------------------------%
f = imread('liftbody.png'); 
%neg transformation
t = imcomplement(f);
%gamma
g = imadjust(f, [ ], [ ], 0.5);
%lograthmic
l = im2uint8(mat2gray(log(1+double(f))));
%ContrastStreching
fd = im2double(f); 
m = mean2(fd) 
c = 1./(1+(m./(fd+eps)).^4); 
%output
figure;
imshow(f);
title('orignal');
figure;
imshow(t);
title('neg trans');
figure;
imshow(g);
title('Gamma');
figure;
imshow(l);
title('lograthmic');
figure;
imshow(c);
title('Contrast');
