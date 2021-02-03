Hello Anton, I'm not trying to change anything, I'm just trying to get a message
across, and apparently all other communication channels (known to me) to the 
st-link project are disabled for outsiders.
                                                                                                                                                            
Please forward this to somebody who's "in charge."                                                                                                                                                      


# To whom it may concern within the `st-link` project                                                                                                                                                                                            
                                                                                                                                                                                                        
I just got locked out of issue #1091, apparently nobody can comment / contact                                                                                                                           
anymore if they have't "contributed to the project in the past." Same goes
for pull requests. I'm not sure if this is by accident, or on purpose. If it's
by accicent, please fix it.                                                                                                                                                                                                 
                                                                                                                                                                                                        
If it's on purpose, it's extremely rude to do that in the middle of a discussion                                                                                                                        
and not even explain *why* it's being done. Moreover, it's not compatible with
the very first line of the Contribution Guideline, which says "We love your input!"
                                                                                                                                                                                                        
Other people, e.g. @Ant-ON, were asking questions, and probably (rightfully)                                                                                                                            
expecting questions which I could be able to provide except for the fact that I                                                                                                                         
cannot post.                                                                                                                                                                                            
                                                                                                                                                                                                        
I just spent an hour investigating and writing a lengthy, pretty detailed email                                                                                                                         
with what I believe to be a bug in `st-link`. And I spent another extra hour                                                                                                                            
researching ways to reach anybody -- person or group -- that I could send that                                                                                                                          
info to, stopping just short of stalking and doxxing the real-life identities                                                                                                                           
and workplace addresses of persons involved.                                                                                                                                                            
                                                                                                                                                                                                        
Here's my contribution, my reply to @Ant-ON (#1091). Please forward.                                                                                                                                    
                                                                                                                                                                                                        
# On issue #1091: not properly connecting                                                                                                                                                               
                                                                                                                                                                                                        
I switched to the `develop` branch. (I also thought that your patch                                                                                                                                     
was integrated, but when it failed, I wanted to try with a version                                                                                                                                      
that I *know* was working.)                                                                                                                                                                             
                                                                                                                                                                                                        
`--connect-under-reset` does the same thing.                                                                                                                                                            
                                                                                                                                                                                                        
Regarding reset I have some extra info: if I reset the device and keep                                                                                                                                  
the reset button pressed, and repeatedly try to `st-info --probe`, I                                                                                                                                    
get different results after few seconds. First is this: 
```
  ...
  flash:      0 (pagesize: 0)                                                                                               
  sram:       0                                                                                                             
  chipid:     0x03e8 
```

Then:
```
  ...
  chipid:     0x0001
```

Then, if I release the reset button, finally:
```
  ...
  chipid:     0x000a
```

And it stays that way. This is true also for the `develop` branch.

Meanwhile I've downloaded the official STM32CubeProgrammer. It also
fails to connect in normal mode, but it *does* connect under
reset. Here's a log of STM32CubeProgrammer, in case it's any help.

```
09:44:06 : ST-LINK SN  : 002A003F3438510C34313939
09:44:06 : ST-LINK FW  : V3J2M1
09:44:06 : Board       : NUCLEO-H743ZI
09:44:06 : Voltage     : 3.26V
09:44:06 : ST-LINK error (DEV_TARGET_CMD_ERR)
09:44:06 : ST-LINK SN  : 002A003F3438510C34313939
09:44:06 : ST-LINK FW  : V3J2M1
09:44:06 : Board       : NUCLEO-H743ZI
09:44:06 : Voltage     : 3.26V
09:44:06 : Error: ST-LINK error (DEV_TARGET_CMD_ERR)
```

Under reset:
```
09:44:14 : ST-LINK SN  : 002A003F3438510C34313939
09:44:14 : ST-LINK FW  : V3J2M1
09:44:14 : Board       : NUCLEO-H743ZI
09:44:14 : Voltage     : 3.26V
09:44:14 : SWD freq    : 24000 KHz
09:44:14 : Connect mode: Under Reset
09:44:14 : Reset mode  : Hardware reset
09:44:14 : Device ID   : 0x450
09:44:14 : Revision ID : Rev V
09:44:14 : UPLOADING OPTION BYTES DATA ...
09:44:14 :   Bank          : 0x00
09:44:14 :   Address       : 0x5200201c
09:44:14 :   Size          : 308 Bytes
09:44:14 : UPLOADING ...
09:44:14 :   Size          : 1024 Bytes
09:44:14 :   Address       : 0x8000000
09:44:14 : Read progress:
09:44:14 : Data read successfully
09:44:14 : Time elapsed during the read operation is: 00:00:00.001
```

The funny part is that the board now, after having connected once
under reset, *always* connects using STM32CubeProgrammer even in
normal mode, while it doesn't using `st-info --probe` (with or without
`--connect-under-reset`). This is even true if I power-cycle the
device!

Let me know if there's anything I can do to help testing.

Cheers
