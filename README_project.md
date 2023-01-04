
Please create an account on Gitlab and give me your username, I will share with you a private project to work on. please push this document renamed into README_Project.md in the branch you're working on, at the root of the repo. Please read this whole file before starting the project. Read it again multiple times if needed, everything is important.

# Goals of the project

a. Fork the ffmpeg official code base. Keep a document of all the git commands used / things you did to achieve that, so that anyone can reproduce your steps by following your instructions

b. Integrate jpegoptim https://github.com/tjko/jpegoptim as a git submodule in the appropriate subfolder for externals resources of ffmpeg

c. add a new line command tool to ffmpeg, for example `ffmpeg -launch_jpegoptim`, with an input .jpg filepath, and all the arguments from jpegoptim

d. when reading the file input we should show the normal ffmpeg information about the inputs

e. we should show (stderr or stdout I don't remember which one, just like normal ffmpeg output in the Terminal) message logs from jpegoptim so that the user using this as a command line can see all information

f. Show in the terminal output how much bytes we saved, filesize before, filesize after, how much percent we shaved off, the time it took and the speed (MegaPixels / seconds)

g. add a new option to parse batch jpg, for example `-input_jpg_folder folder/prefix*suffix.jpg`

h. provide a build of this ffmpeg for the platform you're using (windows x64 or macOS universal binary arm64 + intel x64) so that me or anyone else can try it out ! Also provide explanation as to how to use


 
# General notes about collaboration and work habits

* I'm not looking for the best looking code here, it's just a small test, clean git history is more important
* Do not modify this README.md after pushing it to the repo
* Keep me up to date with what you're working on 
* Respect what you are telling me and if something you told me has changed, warn me about it (for example saying that you are going to work on a specific day then if you don't work on that day, warn me about it instead of leaving me believing you worked on that day)
* Provide build explanation for any external resources or if some part of the project needs to be built to test it
* The repo is not just the code but your work history in general
* Share with me any command line you used (I need to be able to replicate your tests and follow what you do !) or external stuff that is not usually shared in the repo.
* Keeps a journal / logs of what your doings (for example running a command line, save the output of the terminal), even if there are errors. Could be useful for documentation / troubleshooting later on. Especially if you're on a tough problem thinking or testing stuff for hours, keep up a journal about what you're doing so I can follow your work 


Specifically about Git and source code :

* Keep me up to date with what you're working on
* Respect what you are telling me and if something you told me has changed, warn me about it (for example saying that you are going to work on a specific day then if you don't work on that day, warn me about it instead of leaving me believing you worked on that day)
* Please divide your work in small commits otherwise I cannot follow your work. Each action is a commit. If you're pushing one giant commit with everything there is no way for me to know and understand what you are working on
* If you copy pasted code from somewhere else say it in the git commit and comment the code with the URL source. One copy paste = 1 commit then you can modify the code and commit so we can see the differences you added / modified from original example 
commit with everything there is no way for me to know and understand what you are working on
* Don't wait for something to work to commit or push your commits. You can specify in the commit message if something is not working yet, you can have different branches (one stable where it always works and another one where you're trying to make things work, and you can merge-commit from the dev one to the stable one when you think it is stable enough). You can postpone for an hour a commit if you're trying to debug something though (don't forget to explain what you've done in the commit message !)
* Do not hesitate to comment the source code to explain what you're doing or temporary workarounds, but useless comments that repeat variables names should be avoided
* Work on your own branch and tell me when I can test the code (and at which commit ID). I'll merge it onto master or dev myself = no need for a merge request. Please do not use merge requests
* If there are big files to upload it would be best to use Git LFS (it's enabled by default on Gitlab but you need to install this yourself locally !). Git LFS should also be used for most non source code files that aren't going to change a lot (for example .lib and .dll), that way when checking out the repo it only downloads the latest files needed and not the whole history
You need to add the file in .gitattributes for Git LFS to track the file and not regular git. Please make your own Git LFS test on a private repo to be sure it's working for you. If you're not using a Git GUI you might need to initialize it manually (git lfs install, sometimes git lfs pull). If you see the content of a file as a hash then it's probably a Git LFS pointer so it means Git LFS didn't work. Limit is 10 GB per git repo. HUGE files like image sequences and video should rather be stored on a Dropbox / Google Drive we share
* Never delete a branch or rewrite history on the git

