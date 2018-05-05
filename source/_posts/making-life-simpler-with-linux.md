---
title: Making Life Simpler With Linux 🐧
date: 2018-05-05 22:20:47
tags:
  - linux
---
Recently I had to sort some pictures before taking them to the print shop to be printed, so I decided why not mess around and hopefully learn something new in the process :D

<!-- more -->

# Backstory

The event in question was a wedding and I had to make multiple copies of each photo since the same photo would be going to different people.
I wanted to properly sort the pictures in order to make it easier for both the person who will be doing the printing and my self later on when I would have to sort who gets which photographs.

My wife is a photographer and she used to work in shops which printed photographs, so I asked her what is the best way to give the print shop the number of copies I needed from each photo. She recommended the following format because each picture's name shows up on the machine which does the printing

```
{number_of_copies}_{image}.jpg
```

# Solution

I created a folder for each person who would receive the photos, after that, I went through the album and just copied each person to that respective person's folder.

Something like so

```
/media/ddulic/Media/Downloads/Wedding
❯ find . -type f
./Person1/IMG_0490.jpg
./Person1/IMG_0710.jpg
./Person1/IMG_0712.jpg
./Person1/IMG_0777.jpg
./Person2/IMG_0490.jpg
./Person2/IMG_0506.jpg
./Person3/IMG_0490.jpg
./Person3/IMG_0506.jpg
./Person3/IMG_0707.jpg
./Person4/IMG_0490.jpg
./Person4/IMG_0825.jpg
./Person5/IMG_0490.jpg
./Person5/IMG_0627.jpg
./Person5/IMG_0630-2.jpg
./Person5/IMG_0636.jpg
./Person5/IMG_0689.jpg
./Person5/IMG_0694.jpg
./Person5/IMG_0698.jpg
./Person5/IMG_0707.jpg
./Person5/IMG_0710.jpg
./Person5/IMG_0712.jpg
```

Then I copied everything to a different folder where I kept just one image which I will print.

```
/media/ddulic/Media/Downloads/Sorted
❯ find . -type f | while read PIC; do cp $PIC .; done
```

Now I just needed to count the number of copies I wanted for each image, nothing that a few pipes can't solve

```
/media/ddulic/Media/Downloads/Wedding
❯ find . -type f -name '*.jpg" -printf "%f\n" | sort | uniq -c | sort -nr
      6 IMG_0620.jpg
      6 IMG_0506.jpg
      5 IMG_0490.jpg
      4 IMG_0707.jpg
      4 IMG_0694.jpg
      4 IMG_0664.jpg
      3 IMG_0712.jpg
      3 IMG_0552.jpg
      2 IMG_0630-2.jpg
      2 IMG_0627.jpg
      2 IMG_0540.jpg
      1 IMG_0825.jpg
      1 IMG_0535.jpg
```

But that whitespace won't do in our image name, I mean it technically doesn't make a difference but I wanted to remove it.

> Always avoid whitespace when you can :D

Keep in mind that there were a lot more photos then this and I have truncated the output to make it more readable.

The below command renames each photo it finds and adds the number of copies followed by a `_` and the photo number, effectively replacing the whitespace

```
/media/ddulic/Media/Downloads/Sorted
❯ find . -type f -name '*.jpg' -printf "%f\n" | while read PIC; do mv $PIC $(find /media/ddulic/Media/Downloads/Wedding -name $PIC -printf "%f\n" | sort | uniq -c | awk '{print $1}')_$PIC; done
```

A `ls` shows that the files were successfully renamed :3

```
/media/ddulic/Media/Downloads/Sorted
❯ ls
6_IMG_0620.jpg  4_IMG_0694.jpg  2_IMG_0630-2.jpg  1_IMG_0535.jpg
6_IMG_0506.jpg  4_IMG_0664.jpg  2_IMG_0627.jpg
5_IMG_0490.jpg  3_IMG_0712.jpg  2_IMG_0540.jpg
4_IMG_0707.jpg  3_IMG_0552.jpg  1_IMG_0825.jpg
```

# Conclusion

This is just one example out of many, I try and use Linux to play around whenever I can, you should too if you aren't already :)

Got a better solution? Amazing! That is the beauty of Linux, there are multiple solutions to the same problem ^^