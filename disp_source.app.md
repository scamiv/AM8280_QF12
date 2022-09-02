```
disp_source <options> [value]
<options>
-c <target>
	target: string of followings 
	none, hdmi1, hdmi2, vga1, vga2, cvbs, mem, usb1, usb2, network, mirracast, ez_disp, storage, splash
	
-d : detect source 
    
-l : list status 
-s <default_source>: run as service 
	if source changed, the service would show message on console
-o <options>: osd options
   options:
		enable
		disable
		trans_color = <color> 
			to set osd transparent color, the value should reference to the option of osd format type.
		format=<yuv422|rgb565>
			to change osd input data format
		window=<x:y:w:h>
			to change the osd position and size.


-v <options>: VGA function
	options: string of followings 
		auto(re-sync) function  : disp_source.app -v auto
		calibration function    : disp_source.app -v cal
		information function    : disp_source.app -v info

	options=<value>, value can be "0x"(base(16)) or base(10).
		h position          : disp_source.app -v pos=value , value based on timing 
		h relative position : disp_source.app -v pos_offset=value 
		v position          : disp_source.app -v v_pos=value , value based on timing 
		v relative position : disp_source.app -v v_pos_offset=value 
		h size(clock)           : disp_source.app -v clk=value      , value max is 0x7fff
		h relative size(clock)  : disp_source.app -v clk_offset=value  
		phase(tracking) : disp_source.app -v phase=value            , value in [0,63] 
		phase relative  : disp_source.app -v phase_offset=value     
		SOG Threshold   : disp_source.app -v sog_threshold=value    , value in [0,31] 
		3D function     : disp_source.app -v 3D=value               , value = (0:OFF, 2:SbS, 3:TaB, 5:FS)
		ADC Gain function     : disp_source.app -v adc_r_gain=value    , default is 128 and range is [0,255]
		ADC Offset function   : disp_source.app -v adc_r_offset=value  , default is  64 and range is [0,255]
		ADC Load calibrationValue : disp_source.app -v load_adcCalibration 
		Reset SavedModeTable : disp_source.app -v reset_savedModeTable 
		Print SavedModeTable : disp_source.app -v print_savedModeTable 
		Auto Optimization     : disp_source.app -v auto_optimization=value      , value = (0:disable, 1:timing, 2:always)
		Internal TestPattern     : disp_source.app -v testpattern=value    
           , value = (0:gray scale 256, 1:color bar, 2:border, 6:All color, 7:chess box, 8:ramp gray, 9:ramp color, 10:true color, 11:ramp v, 12:grill 11, 13:grill 22, 14: grill 44, 15: grill 88)
		Auto Apply 3D function:
		disp_source.app -v auto3Dhres=value,auto3Dvres=value,auto3Dframerate=value,auto3Dvideotype=value, auto3Dmode=value
			auto3Dhres is the h resoultion of the target timing
			auto3Dvres is the v resoultion of the target timing
			auto3Dframerate is the frame rate of the target timing
			auto3Dvideotype is the video type  (0:Progressive, 1:Interlace)
			auto3Dcolrospace is the color space  (0:RGB, 1:YUV)
			auto3Dmode is the 3D mode  (0:OFF, 1:FP, 2:SbS, 3:TaB, 4:Auto, 5:FS)
	example: 
			disp_source.app -v auto3Dhres=1920,auto3Dvres=1080,auto3Dframerate=60,auto3Dvideotype=1,auto3Dcolorspace=1,auto3Dmode=2


-h <options>: HDMI function
	options: string of followings 
		re-sync function        : disp_source.app -h resync
		information function    : disp_source.app -h info
		manual csc mode select  : disp_source.app -h manualcscmode=value  ,value in [0,2], 0:auto , 1:rgb, 2:yuv
	options=<value>, value can be "0x"(base(16)) or base(10).
		overscan    : disp_source.app -h overscan=value     , value = (0:Auto, 1:Underscan, 2:Overscan)
		color_range : disp_source.app -h color_range=value  , value = (0:Auto, 1:Limited, 2:Full)
		3D function : disp_source.app -h 3D=value           , value = (0:OFF, 1:FP, 2:SbS, 3:TaB, 4:Auto, 5:FS)
		SW EQ       : disp_source.app -h sw_eq=value        , value range is [0,6], 7: auto, 8: default 
		HPD ctrl    : disp_source.app -h hpd_ctrl=value     , value = (0: HPD is Low, 1: HPD is High) 
		Auto Apply 3D function:
		disp_source.app -h auto3Dhres=value,auto3Dvres=value,auto3Dframerate=value,auto3Dvideotype=value, auto3Dmode=value
			auto3Dhres is the h resoultion of the target timing
			auto3Dvres is the v resoultion of the target timing
			auto3Dframerate is the frame rate of the target timing
			auto3Dvideotype is the video type  (0:Progressive, 1:Interlace)
			auto3Dcolrospace is the color space  (0:RGB, 1:YUV)
			auto3Dmode is the 3D mode  (0:OFF, 1:FP, 2:SbS, 3:TaB, 4:Auto, 5:FS)
	example: 
			disp_source.app -h auto3Dhres=1920,auto3Dvres=1080,auto3Dframerate=60,auto3Dvideotype=1,auto3Dcolorspace=1,auto3Dmode=2


-a <target>
	audio target: string of followings 
	none, hdmi1, hdmi2, vga1, vga2, cvbs, mem, usb1, usb2, network, mirracast, ez_disp, storage


-x <options>: Splash function
	options=<value>, value can be "0x"(base(16)) or base(10).
		disp_source.app -splash load=splash.bin width=1920 height=1080 pixelbytes=3     
			splash.bin is the hex file
			width is the image width
			height is the image height
			pixelbytes is the bytes per pixel  (YUV422:2, CRGB:6,RGB10B:7 )
	example: 
			disp_source.app -c splash 
			disp_source.app -x load=logo.log,width=1920,height=1080,pixelbytes=2 
	options=<path>, load bmp file
	example: 
			disp_source.app -c splash 
			disp_source.app -x bmp=./logo.bmp  
      
```
