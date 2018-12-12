## VHDL Notes

##### Required Libraries
```
library ieee;
use ieee.std_logic_1164.all;
```

##### Comments
```
-- This is a Comment
```

##### Entity Declarations
All the VHDL designs are implemented by entity. You can imagine the entity as a black box with input and output ports.

```
entity my_entity is
port(
 in_port1 : in std_logic;
 in_port2 : in std_logic_vector( 7 downto 0);

 out_port1 : out std_logic;
 out_port2 : out std_logic_vector(7 downto 0));
end my_entity;
```

##### Architectures

```
architecture architecture_name of entity_name is
	declarations
begin
	concurrent statements
end architecture_name;
```

![Arch graphic](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/entity-architecture_and2.jpg)
