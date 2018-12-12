## ELEC240 - Memory

#### ROM - Read Only Memory
- Masked ROM
- PROM - Programmable ROM
- EPROM - Erasable PROM
- EEPROM - Electrically erasable PROM
- Flash - NAND & NOR

#### RAM - Random Access Memory
- SRAM - Static RAM
- DRAM - Dynamic RAM

#### Memory Structure

![memory stucture](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/memoryStruc.jpg)

------------
### ROM
Masked prom are programmed at the time of manufacture with a special mask. This is a lost cost method when more than 100,000 are required. The major drawback is that once programmed there is no way of changing the information stored.


#### PROM
Made up of an array of fuses thus can only be programmed once. Programming is accomplished with a current (instead of voltage as in EPROMs) that blows the fuses.

![prom array](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/promArray.jpg)


###### Reading PROM
To read data an address is presented and the decoder will activate the required row by forcing the row select signal low. All other row selects will remain high and so the diodes will be reversed biased.

On the selected row the presence of a fuse causes the corresponding data line to be pulled low resulting in a '0'  at the output. If no fuse is present then the pull-up resistor means a '1' will be seen at the output.

![ prom operation](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/promArrayEDIT.jpg)

###### Writing PROM
To program a PROM device which has all the fuses present an address is presented causing a row to be selected. Then the data required is presented on the data lines from a low impedance source. This causes a high current to flow which blows the fuse.

#### EPROM
Can be programmed and erased via UV light exposure. Instead of using fuses to either short a bit ('0') or let the voltage pass to the output buffer ('1') EPROM cells use a **floating gate**.

![floating gate operation](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/floating_gates.gif)

###### Writing EPROM
The EPROM device is programmed by first selecting the row which makes the row select go 'high' not low like with PROM. Then, by forcing an electrical charge onto the floating gate (high voltage Vpp). When this charge is present on the gate the cell is 'programmed', usually to a logic '0'. When the charge is not present it is a logic '1'. When the EPROM chip is cleared all the bits are '1'. You program the '0'.

![cell function](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/floating-gate-cell.png)

The movement of electrons on the floating gate is due to a phenomenon called *hot electron injection*. High voltages are applied to the select gate and drain connections. The select gate is then pulsed high causing a large current to flow. The large bias voltage on the gate connection attracts electrons that penetrate the thin gate oxide and are stored on the floating gate.  

EPROM is then read in the same way as for the PROM.

#### EEPROM
EEPROM is similar to EPROM but can be erased using an electric field instead of UV. This eliminates the need for a ceramic package which reduces the cost greatly as a plastic package can be used. With EEPROM it is possible to read, write and, most importantly, **re-write individual bytes**. EEPROM can be programmed in circuit by applying special signals and can be programmed up to 1 million times. EEPROM takes more die area than flash memory for the same capacity because each cell requires two transistors. One for read and another for write/ erase.

 There are multiple ways to implement EEPROM memory cells but the most common is composed of two transistors. The storage transistor has a floating gat that will trap electrons. In addition there is a access transistor which is required for read/write operation.

An **EPROM** cell is erased when the electrons are removed from the floating gate cell and a **EEPROM** cell is erased when the electrons are trapped on the floating gate. As with EPROM, EEPROM will be read as a '1' for an erase state and a '0' for a programmed state.

### Flash
Flash memory is also a non-volatile memory that is similar to EEPROM. However, Flash memory is **erased and programmed in blocks**, not in bytes like EEPROM. A write operation can only be performed on an empty /erased unit so in most cases an erase must precede a write. Flash is much cheaper than EEPROM and has therefore become the dominant memory type used when a significant amount of storage space is required (SSD for example).

![ nor nand](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/nor-nand.png)

#### NOR Flash
Nor flash has long erase and write times but provides full address and data buses, allowing for **random access to any memory location**. This makes NOR suitable to replace older ROM chips which stores program code that rarely needs to be updated (BIOS). Because of this, microprocessors can use NOR flash   as execute in place (XIP) memory to run programs directly from memory.

Because the floating gate is isolated, any electrons placed there via Fowler-Nordheim tunneling are trapped there and under normal conditions will not discharge for many years. When the floating gate holds a charge it screens the electric field from the control gate of the FET. This modifies the threshold voltage of the cell. Voltage applied to the control gate and the FET channel will become conducting or remain insulated, depending on the threshold voltage of the cell.

When there are **no electrons** on the floating gate the cell is considered to be in an **erased state**. An erased state represents a bit value of '1'. When there is a charge on the floating gate it is in a programmed state and it will be read as a bit value '0'.

##### Writing to NOR Flash
A NOR cell is in its default of state of bit vale '1' when unprogrammed. This is because current will flow through the channel when a voltage is applied to the control gate.

A NOR cell can be programmed (set to '0') by:
- applying a voltage to the control gate
- The channel is now turned on so electrons can flow from the source to drain (assuming NMOS).
- The source-drain current is high enough to cause some electrons to jump through the insulated layer onto the floating gate via hot electron injection.

##### Erasing NOR Flash
To erase a NOR flash cell (set to '1'), a large negative voltage is applied between the control gate and source, pulling the electrons off the floating gate through quantum tunneling. NOR chips are divided into erase segments. The erase can only be performed on a block-wise basis. Programming can be done one byte at a time However

##### NOR Key Points
- **Written in BYTES**
- **Erased in BLOCKS**
- **Random access to any location**
- **Slow erase and write times**
- **Finite number of Read/Write cycles**

#### NAND Flash
NAND flash uses tunnel injection for writing and tunnel release for erasing. Used in USB storage devices due to price and fast write/erase in comparison to NOR. NAND is accessed in blocks often called pages. While reading and programming is performed on a page basis, erase can only be performed on a block basis. The largest limitation of NAND is data in a block can only be written sequentially.

![comparasion](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/nor_nand_comp.jpg)
