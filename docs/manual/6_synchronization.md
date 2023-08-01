
# How to synchronize your devices

If you use our external trigger, you can use its inputs, to synchronize your devices.
Most of the supported microcontroller boards log the inputs at pins 16-23 into a <recording_path>/triggerdata.csv file.

For time synchronization, the microcontroller boards log the first inputs at the time the first pulse was sent. This way you can synchronize the first images of the cameras and their internal clock to the internal clock of the external trigger.

```text
                           ┌─────┐     ┌─────┐     ┌─       ─┐
         ...               │     │     │     │     │   ...   │ ...
              ─────────────┘     └─────┘     └─────┘         └───────────
wavestate           0      │  1  │  0  │  1  │  0  │ 1     n │     0
pulse_count     UINT32_MAX │  0  │  0  │  1  │  1  │ 2     n │ UINT32_MAX
pulse_count (cont.) n      │ n+1 │ n+1 │ n+2 │ n+2 │n+3   n+m│    n+m
sync_rising_edge == true   ┴     │
sync_rising_edge == false        ┴
```

The file contents will look similar to this:

`metadata.csv`

```csv
frame_camera_serial ;frame_camera_name ;frame_id ;frame_timestamp ;frame_image_uid
           19415034 ;Camera_4          ;       0 ;597449368203896 ; 94017979513872
           19415037 ;Camera_3          ;       0 ;163273368203367 ; 94017967892976
           19415033 ;Camera_1          ;       0 ;272583328921019 ; 94017972152896
           21013374 ;Camera_0          ;       0 ;770316960921060 ; 94017962792256
           19415035 ;Camera_2          ;       0 ;855758496921019 ; 94017968796096
           19415037 ;Camera_3          ;       1 ;163273382493569 ; 94017975321952
           19415033 ;Camera_1          ;       1 ;272583343210976 ; 94017978831696
           19415035 ;Camera_2          ;       1 ;855758511211505 ; 94017973880432
           21013374 ;Camera_0          ;       1 ;770316975211505 ; 94017966449648
           19415034 ;Camera_4          ;       1 ;597449382494016 ; 94017981830272
           19415037 ;Camera_3          ;       2 ;163273396783772 ; 94017979597136
           21013374 ;Camera_0          ;       2 ;770316989501708 ; 94017966467888
           19415033 ;Camera_1          ;       2 ;272583357501546 ; 94017995748160
           19415035 ;Camera_2          ;       2 ;855758525501505 ; 94017978237504
           19415034 ;Camera_4          ;       2 ;597449396784219 ; 94017990808464
...
```

`triggerdata.csv`

```csv
flag_0 ;flag_1 ;flag_2 ;flag_3 ;flag_4 ;flag_5 ;flag_6 ;flag_7 ;pulse_id ;uptime_us
true   ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;       0 ;2508068633
false  ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;   57822 ;3086494592
true   ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  155876 ;4067383774
false  ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  155876 ;4067383785
true   ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  156850 ;4077121115
false  ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  163316 ;4141801073
true   ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  163316 ;4141801084
false  ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  167740 ;4186060281
true   ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  171554 ;4224218818
false  ;true   ;true   ;true   ;true   ;true   ;true   ;true   ;  172716 ;4235837376
...
```

If your setup is working correctly, pulse_id and frame_id should also align within their time intervals.

Please note that if you rely on the external trigger or the in-camera clocks, you will need to correct them for longer recordings as they will drift away from each other, since they are not real-time clocks. Also, unfortunately, the trigger uptime is currently only a 32-bit unsigned integer, which means it overflows every 71 minutes and 35 seconds.
