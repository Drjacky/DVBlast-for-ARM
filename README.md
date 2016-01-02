# DVBlast-for-ARM
How to compile dvblast from source code for arm processors


It's assumed you installed gcc arm; But if you didn't:
sudo apt-get install gcc-arm-linux-gnueabi
sudo apt-get install g++-arm-linux-gnueabi
sudo apt-get install build-essential

Then get dvblast source code:
wget https://get.videolan.org/dvblast/3.0/dvblast-3.0.tar.bz2

dvblast has one build dependancy (bitStream):
sudo apt-get install ttf-bitstream-vera

But above command install bitstream for default gcc; We want it for our gcc arm:
cp -R /usr/local/include/bitstream /usr/arm-linux-gnueabi/include/

Now you should edit dvblast makefile to add cross-compile feature to it:

%.o: %.c Makefile dvblast.h en50221.h comm.h asi.h mrtg-cnt.h
	@echo "CC      $<"
	$(Q)$(CC) $(CFLAGS) -c $<

to

%.o: %.c Makefile dvblast.h en50221.h comm.h asi.h mrtg-cnt.h
	@echo "CC      $<"
	$(Q)$(CROSS)$(CC) $(CFLAGS) -c $<



dvblast: $(OBJ_DVBLAST)
	@echo "LINK    $@"
	$(Q)$(CC) -o $@ $(OBJ_DVBLAST) $(LDLIBS_DVBLAST) $(LDLIBS)

to

dvblast: $(OBJ_DVBLAST)
	@echo "LINK    $@"
	$(Q)$(CROSS)$(CC) -o $@ $(OBJ_DVBLAST) $(LDLIBS_DVBLAST) $(LDLIBS)


dvblastctl: $(OBJ_DVBLASTCTL)
	@echo "LINK    $@"
	$(Q)$(CC) -o $@ $(OBJ_DVBLASTCTL) $(LDLIBS_DVBLAST) $(LDLIBS)

to

dvblastctl: $(OBJ_DVBLASTCTL)
	@echo "LINK    $@"
	$(Q)$(CROSS)$(CC) -o $@ $(OBJ_DVBLASTCTL) $(LDLIBS_DVBLAST) $(LDLIBS)


Now time to compile:
make CROSS=arm-linux-gnueabi-g

*NOTE:
At the end of command, it's 'g', not'gcc'.

After compile, if you want to check everything is correct:
file dvblast
