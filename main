import os
import sys
import cv2
from os import listdir
from os.path import isfile, join
import natsort 
import imageio

src = "/media/ee401_2/Didik/demo_most/mov/"
save_rgb = "/media/ee401_2/Didik/demo_most/rgb/"
os.makedirs(save_rgb) if not os.path.exists(save_rgb) else print('rgb folder exists!')
save_gif = "/media/ee401_2/Didik/demo_most/gif/"
os.makedirs(save_gif) if not os.path.exists(save_gif) else print('gif folder exists!')

strat_time = 4
end_time = 16


def main():
	vid = [f for f in listdir(src) if isfile(join(src, f))]
	for line in vid:
		path = os.path.join(src, line)
		print(path)
		cam = cv2.VideoCapture(path)
		fps = cam.get(cv2.CAP_PROP_FPS)
		currentframe = 0
		count = 0

		os.makedirs(os.path.join(save_rgb, line[:-4])) if not os.path.exists(
			os.path.join(save_rgb,line[:-4])) else print(line, ' exists!')

		os.makedirs(os.path.join(save_rgb, line[:-4] + '_LR')) if not os.path.exists(
			os.path.join(save_rgb,line[:-4] + '_LR')) else print(line, ' exists!')

		while(True): 
		    ret,frame = cam.read() 
		    if(ret): 
		        if currentframe >= strat_time*fps and currentframe <= end_time*fps:
		        	img = cv2.resize(frame, (320, 240)) 
		        	cv2.imwrite(os.path.join(os.path.join(
		        		save_rgb, line[:-4]), str(count).zfill(6) + '.jpg'), img)
		        	
		        	img_LR = cv2.resize(img, (16, 12)) 
		        	img_LR = cv2.resize(img_LR, (320,240), interpolation = cv2.INTER_AREA)
		        	cv2.imwrite(os.path.join(os.path.join(
		        		save_rgb, line[:-4] + '_LR'), str(count).zfill(6) + '.jpg'), img_LR)

		        	if count % (fps*3) == 0:
		        		print('writing images ... ')
		        	count += 1
		        currentframe += 1
		    else:
		    	break		  
		cam.release()
		print('1/2 finished')

		filenames = [fn for fn in os.listdir(os.path.join(save_rgb, line[:-4])) if fn.endswith('.jpg')]
		filenames = natsort.natsorted(filenames,reverse=False)
		
		file_output = os.path.join(save_gif, line[:-4] +'.gif')
		file_output_LR = os.path.join(save_gif, line[:-4] +'_LR.gif')

		with imageio.get_writer(file_output, mode='I', duration=0.1) as writer:
			for filename in filenames:
			    image = imageio.imread(os.path.join(save_rgb, line[:-4], filename))
			    writer.append_data(image)

		with imageio.get_writer(file_output_LR, mode='I', duration=0.1) as writer:
			for filename in filenames:			    
			    image_LR = imageio.imread(os.path.join(save_rgb, line[:-4] + '_LR', filename))
			    writer.append_data(image_LR)
		print('2/2 finished')

main()

