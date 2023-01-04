Hi.
Thank you for visiting this repository.

This project is based on ffmpeg and jpegoptim.

You can check more details on https://github.com/tjko/jpegoptim,  https://github.com/FFmpeg/FFmpeg

In this project, I integrated ffmpeg and jpegoptim, and modified some code part to achieve test purposes.


Working Environment : 
	
	Windows 10 (x64), Visual Studio.

If you use linux or mac OS, it will be easier to build projects.


How to build Visual Studio Project from ffmpeg, jpegoptim.


1. download ffmpeg official release.(https://github.com/FFmpeg/FFmpeg)

2. Generate Visual studio project from ffmpeg.

    There are many urls about that.

	https://trac.ffmpeg.org/wiki/CompilationGuide/MSVC

	https://trac.ffmpeg.org/wiki/CompilationGuide/MinGW

	https://github.com/ShiftMediaProject/FFVS-Project-Generator

	https://github.com/mcmtroffaes/ffmpeg-msvc-build

3. I used https://github.com/ShiftMediaProject/FFVS-Project-Generator.

	Because it asks no other tools except visual studio.
	
	please read carefully https://github.com/ShiftMediaProject/FFVS-Project-Generator README.md.
	
	They ask to install VSNASM. Nasm used for build some ffmpeg binaries like avfilter.
	
4. Compile FFVS project first and generated EXE file from that.

5. Generate Visual studio project from ffmpeg. (you can use command line arguments.)

6. Open generated VS project and try to compile.

7. Build success and you can see some *.lib files like avdevice, avfilter, ... and some .exe files like ffprobe, 
   
   ffmpeg, some .dll files like swscale.dll.

8. We focus ffmpeg/fftools. This is the code part for ffmpeg.exe and we need to modify it.

9. Download jpegoptim.

	For convenience we use https://github.com/XhmikosR/jpegoptim-windows rather than
	
	https://github.com/tjko/jpegoptim.
	
	https://github.com/XhmikosR/jpegoptim-windows include https://github.com/tjko/jpegoptim as a submodule. 
	
	it contains only windows visual studio project files more.
	
10. jpegoptim-windows include mozjpeg library. 

11. Add jpegoptim vcxproj project to ffmpeg visual studio project.

12. we are ready to modify source code.


My Work : 


1. Modify options in ffmpeg_opt.c to support `ffmpeg -launch_jpegoptim`.

   ffmpeg_opt_c defines commandline argument for ffmpeg. at the bottom of the file you can see
   
   const OptionDef options[] = { 
   
   ...}
   
   This is command line argument definitions and we need to add some lines for launch_jpegoptim.
   
![image](https://user-images.githubusercontent.com/112364526/209586144-ae9adc98-c847-4e5d-bfee-58697ac1f81a.png)

2. Find code to modify for -launch_jpegoptim

   In main()(ffmpeg.c) function calls ffmpeg_parse_options(ffmpeg_opt.c) to parse command line argument of ffmpeg.
   
   This function parse options and open all input/output files.
   
   and ffmpeg_parse_options(ffmpeg_opt.c) calls many functions like split_commandline, parse_optgroup, term_init,
   
   open_files, ... etc.
   
   I will modify split_commandline(cmdutils.c) function.
   
3. Modify split_commandline function to support -launch_jpegoptim and others

   split_commandline function check every commandline arugment and make options.
   
   I added `if` statement to check -launch_jpeg_optim.
   
   ![image](https://user-images.githubusercontent.com/112364526/210022950-6dbcff5a-cd74-4a32-a6ca-bed650ef1d26.png)

   po->name is commandline argument. -launch_jpeg_optim means you have to use jpegoptim functions.
   
   if we meet this argument, we pass all parameters to the jpegoptim executable. It was done by calling CreateProcess.
   
   check Batch processing(--input_jpg_folder) and make input to ffmpeg open_file function.
   
   ffmpeg's open_file function parse input files and output data.


How to check result : 


1. Download my work by git clone or download as zip.(from Github or GitLab)

2. Open ffmpeg.sln file in VS directory by Visual Studio. I am using Visual Studio 2019.

3. For simplify our project include only ffmpeg, jpegoptim.

4. Build jpegoptim first. It generate jpegoptim.exe under the ../../msvc directory.

5. Try to build ffmpeg. It generates some link errors.

6. Because I never included projects like avdevices, avfilter, avformat, avcodec,.. so you have to include it manually.

   Fortunately, you can get that lib and dll files easily on google.

7. add that files to input dependenices in visual studio and build again. It will be success.

8. Try to check.

![image](https://user-images.githubusercontent.com/112364526/209586315-8fdcfdba-ae90-4002-ade0-dc8092d2d28a.png)

ffmpeg -launch_jpegoptim --preserve --strip-all --totals --verbose E:/2022/msvc/jpg/1000.jpg

![image](https://user-images.githubusercontent.com/112364526/209586337-867004ba-0d13-4a88-b674-b512bdf3ffb2.png)

ffmpeg -launch_jpegoptim --preserve --strip-all --totals --input_jpg_folder --verbose E:/2022/msvc/jpg/*.jpg

ffmpeg -launch_jpegoptim --preserve --strip-all --totals --input_jpg_folder --verbose E:/2022/msvc/jpg/1*.jpg

ffmpeg -launch_jpegoptim --preserve --strip-all --totals --input_jpg_folder --verbose E:/2022/msvc/jpg/*abc.jpg

ffmpeg -launch_jpegoptim --preserve --strip-all --totals --input_jpg_folder --verbose E:/2022/msvc/jpg/1*abc.jpg

ffmpeg -launch_jpegoptim --preserve --strip-all --totals --input_jpg_folder --verbose E:/2022/msvc/jpg

![image](https://user-images.githubusercontent.com/112364526/209586329-311c5b4f-0302-4646-b1a5-f18d93ded159.png)


PS :


1. ffmpeg_jpegoptim_windows/jpegoptim-prj/src is submodule

2. ffmpeg.exe and jpegoptim.exe are built binaries that you can check.
