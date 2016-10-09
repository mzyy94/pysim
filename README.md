# pysim

This utility allows to :

* Create EAP-SIM authentication entry for FreeRADIUS *users* file:

  for example
  ```
  ./pySim-gen-eapsim-user.py | sudo tee -a /etc/freeradius/users
  ```
  
  > NOTE: If you use PC/SC reader instead of serial connected one, run it with *-p* option.  

* Program customizable SIMs. Two modes are possible:

  - one where you specify every parameter manually :

  ```
  ./pySim-prog.py -n 26C3 -c 49 -x 262 -y 42 -i <IMSI> -s <ICCID>
  ```

  - one where they are generated from some minimal set :


  ```
  ./pySim-prog.py -n 26C3 -c 49 -x 262 -y 42 -z <random_string_of_choice> -j <card_num>
  ```
  
  > With **<random_string_of_choice>** and **<card_num>**, the soft will generate
    'predictable' IMSI and ICCID, so make sure you choose them so as not to
    conflict with anyone. (for eg. your name as **<random_string_of_choice>** and
    0 1 2 ... for **<card num>**).

  You also need to enter some parameters to select the device :
   -  -t TYPE : type of card (supersim, magicsim, fakemagicsim or try 'auto')
   -  -d DEV  : Serial port device (default /dev/ttyUSB0)
   -  -b BAUD : Baudrate (default 9600)

* Interact with SIMs from a python interactive shell (ipython for eg :)

```python
from pySim.transport.serial import SerialSimLink
from pySim.commands import SimCardCommands

sl = SerialSimLink(device='/dev/ttyUSB0', baudrate=9600)
sc = SimCardCommands(sl)

sl.wait_for_card()

	# Print IMSI
print sc.read_binary(['3f00', '7f20', '6f07'])

	# Run A3/A8
print sc.run_gsm('00112233445566778899aabbccddeeff')
```
