# DVBlast-for-ARM
How to compile dvblast from source code for arm processors


It's assumed you installed gcc arm; But if you didn't:
```javascript
sudo apt-get install gcc-arm-linux-gnueabi
sudo apt-get install g++-arm-linux-gnueabi
sudo apt-get install build-essential
```
Then get dvblast source code:
```javascript
wget https://get.videolan.org/dvblast/3.0/dvblast-3.0.tar.bz2
```
dvblast has one build dependancy (bitStream):
```javascript
sudo apt-get install ttf-bitstream-vera
```
But above command install bitstream for default gcc; We want it for our gcc arm:
```javascript
cp -R /usr/local/include/bitstream /usr/arm-linux-gnueabi/include/
```
Now you should edit dvblast makefile to add cross-compile feature to it:
```javascript
%.o: %.c Makefile dvblast.h en50221.h comm.h asi.h mrtg-cnt.h
	@echo "CC      $<"
	$(Q)$(CC) $(CFLAGS) -c $<
```
to
```javascript
%.o: %.c Makefile dvblast.h en50221.h comm.h asi.h mrtg-cnt.h
	@echo "CC      $<"
	$(Q)$(CROSS)$(CC) $(CFLAGS) -c $<
```
______________________________________________________________
```javascript
dvblast: $(OBJ_DVBLAST)
	@echo "LINK    $@"
	$(Q)$(CC) -o $@ $(OBJ_DVBLAST) $(LDLIBS_DVBLAST) $(LDLIBS)
```
to
```javascript
dvblast: $(OBJ_DVBLAST)
	@echo "LINK    $@"
	$(Q)$(CROSS)$(CC) -o $@ $(OBJ_DVBLAST) $(LDLIBS_DVBLAST) $(LDLIBS)
```
______________________________________________________________
```javascript
dvblastctl: $(OBJ_DVBLASTCTL)
	@echo "LINK    $@"
	$(Q)$(CC) -o $@ $(OBJ_DVBLASTCTL) $(LDLIBS_DVBLAST) $(LDLIBS)
```
to
```javascript
dvblastctl: $(OBJ_DVBLASTCTL)
	@echo "LINK    $@"
	$(Q)$(CROSS)$(CC) -o $@ $(OBJ_DVBLASTCTL) $(LDLIBS_DVBLAST) $(LDLIBS)
```

Now time to compile:
```javascript
	make CROSS=arm-linux-gnueabi-g
```
*NOTE:
At the end of command, it's 'g', not'gcc'.

After compile, if you want to check everything is correct:
file dvblast
