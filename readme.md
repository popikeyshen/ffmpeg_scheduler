## Make mp4 video with FFmpeg and trace functions


### Part 1 - compile ffmpeg with 

```
git clone https://github.com/FFmpeg/FFmpeg
cd FFmpeg/
sudo apt install -y libx264-dev
sudo apt install -y libx265-dev
./configure --enable-static  --enable-libx264 --enable-gpl
make
```

Enagle gpl or you become "libx264 is gpl and --enable-gpl is not specified."

Install libx264 dev or you become "ERROR: x264 not found using pkg-config"




### Part 2 - Add print

In file fftools/ffmpeg.c is function main()
There is created the scheduler
```
    Scheduler *sch = NULL;
    ...
    sch = sch_alloc();
```

To end of main() before the scheduler will be deleted need to add and print function that will show parameters of shed
```
// add this to main() in fftools/ffmpeg.c
print_shed(sch)
```

The scheduler is in file fftools/ffmpeg_sched.c
Where we need to add this print 

```

// add this function to fftools/ffmpeg_sched.c before struct Scheduler {
void print_shed(Scheduler *sch)
{

	printf("\nprint from shed.c\n");
	printf("\n first param %d \n",sch->sdp_auto);
	printf("\n second param %s \n",sch..sdp_filename);
	// make all code here
	
}

struct Scheduler {
    const AVClass      *class;

    SchDemux           *demux;
    unsigned         nb_demux;

    SchMux             *mux;
    unsigned         nb_mux;
...
```



### Part 3 - Run 

```
./ffmpeg -r 1/2 -start_number 0 -i ./images/%02d.jpg -c:v libx264 -r 30 -pix_fmt yuv420p out.mp4
```

