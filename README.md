<h2  align="center" id="physfs">What is PhysicsFS?</h2>

PhysicsFS (short for Physics File System) is a library that provides a platform-independent abstraction layer for accessing various archive formats, such as ZIP, TAR, and ISO files, as well as directories and virtual filesystems. It was originally designed for use in video games to make it easier for developers to manage game assets, but it can be used for any software that needs to access files stored in archives.

Providing abstract access means that the PhysicsFS library provides a standardized interface or set of functions that allow developers to access files stored in various archive formats in a uniform way, regardless of the underlying details of those archives.

For example, us who are developers creating a videogame who want to load some sound files. Those sound files could be stored in a variety of different archive formats, such as ZIP, RAR, or 7z. Without a library like PhysicsFS, we would need to write code to handle each of these archive formats individually, which could be complex and time-consuming.

However, by using PhysicsFS, we can write a single piece of code that works with all of these archive formats in a consistent way. This is because PhysicsFS provides a set of functions that abstract away the details of working with each individual archive format, and instead provides a single, unified interface for accessing files stored in archives. This makes it easier for us to work with file archives and can help to make our software more portable and maintainable.



<h2  align="center" id="physfs">We will use it</h2>

<p align="justify">Using PhysFS involves loading absolutely all files through this API, in a different way than conventional. That means that we must understand how the necessary functions that PhysicsFS brings. The following functions are those that allow us to start working, with which the project will be opened and closed:</p> 

<ul>
  <li><b>PHYSFS_init:</b> Initialize the PhysicsFS library. This must be called before any other PhysicsFS function.</li>


  <li><b>PHYSFS_deinit:</b> Deinitialize the PhysicsFS library. This closes any files opened via PhysicsFS, blanks the search/write paths, frees memory, and invalidates all of your file handles.</li>


  <li><b>PHYSFS_mount:</b> Add an archive or directory to the search path. If this is a duplicate, the entry is not added again, even though the function succeeds. You may not add the same archive to two different mountpoints: duplicate checking is done against the archive and not the mountpoint.</li>


  <li><b>PHYSFS_close:</b> Close a PhysicsFS filehandle.This call is capable of failing if the operating system was buffering writes to the physical media, and, now forced to write those changes to physical media, can not store the data for some reason.</li>
</ul>


<h3  align="center" id="abmount">About Mounting</h3>

<p align="justify">Mounting a file is very important in an API like this, which allows you to carry out the rest of the functionalities.</p> 
		
<p align="justify">When you mount an archive, it is added to a virtual file system...all files in all of the archives are interpolated into a single hierachical file tree. Two archives mounted at the same place (or an archive with files overlapping another mountpoint) may have overlapping files: in such a case, the file earliest in the search path is selected, and the other files are inaccessible to the application. This allows archives to be used to override previous revisions; you can use the mounting mechanism to place archives at a specific point in the file tree and prevent overlap; this is useful for downloadable mods that might trample over application data or each other, for example.</p> 

<p align="justify">The mountpoint does not need to exist prior to mounting, which is different than those familiar with the Unix concept of "mounting" may expect. As well, more than one archive can be mounted to the same mountpoint, or mountpoints and archive contents can overlap...the interpolation mechanism still functions as usual.</p> 


<h3  align="center" id="nefunctions">Necessary Functions</h3>

<p align="justify">Once we have initialized the API and have mounted a virtual file, we will go to other functions. Among them, the most important will be listed below:</p> 

<ul>
  <li><b>PHYSFS_openRead:</b> Open a file for reading. Open a file for reading, in platform-independent notation. The search path is checked one at a time until a matching file is found, in which case an abstract filehandle is associated with it, and reading may be done. The reading offset is set to the first byte of the file.</li>


  <li><b>PHYSFS_fileLength:</b> Get total length of a file in bytes. Note that if another process/thread is writing to this file at the same time, then the information this function supplies could be incorrect before you get it. </li>
  
  
   <li><b>PHYSFS_eof:</b> Check for end-of-file state on a PhysicsFS filehandle. Determine if the end of file has been reached in a PhysicsFS filehandle.</li>


  <li><b>PHYSFS_read:</b> Reads from a PhysicsFS filehandle. The file must be opened for reading.</li>
</ul>


<h3  align="center" >Important concepts that you might not know</h3>

<ul>
  <li><b>Virtual file system:</b> Basically a virtual file system is a virtual folder in which tou can access many different folder regardless of their physical position in the computer</li>


  <li><b>Buffer:</b> It's a temporal zone of the memory that is used to store data that is being transfered from one part of the program to another. An example is when a program is reading a file, like in our case with the PhysFS_read functiob.</li>
</ul>




<h3  align="center" id="nesdlfunctions">Necessary SDL Functions</h3>

<p align="justify">To upload files with this new methodology, we will not be able to use the same upload functions with SDL. In this way, from now on, we must know <b>SDL_RWops</b> and all its derivatives.</p> 

<p align="justify"> SDL_RWops is a data structure that is used to manipulate I/O files. it is used to do operationsn of reading and wirting in different file such as binary files, audio video and more. so with this that structure we won't need to take care of any of the specific details of every file. Also it allows you to read and write in copmpressed or encypted files.
</p> 

<p align="justify">The functions we currently use should be replaced by:</p> 

<ul>
  <li><b>IMG_Load_RW</b></li>


  <li><b>Mix_LoadWAV_RW</b></li>
  
  
   <li><b>Mix_LoadMUS_RW</b></li>
</ul>
