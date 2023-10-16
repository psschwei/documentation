---
title: qpy
description: API reference for qiskit.qpy
in_page_toc_min_heading_level: 1
python_api_type: module
python_api_name: qiskit.qpy
---

<span id="module-qiskit.qpy" />

<span id="qiskit-qpy" />

<span id="qpy-serialization-qiskit-qpy" />

# QPY serialization

<span id="module-qiskit.qpy" />

`qiskit.qpy`

QPY is a binary serialization format for [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") and [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock") objects that is designed to be cross-platform, Python version agnostic, and backwards compatible moving forward. QPY should be used if you need a mechanism to save or copy between systems a [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") or [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock") that preserves the full Qiskit object structure (except for custom attributes defined outside of Qiskit code). This differs from other serialization formats like [OpenQASM](https://github.com/openqasm/openqasm) (2.0 or 3.0) which has a different abstraction model and can result in a loss of information contained in the original circuit (or is unable to represent some aspects of the Qiskit objects) or Python’s [pickle](https://docs.python.org/3/library/pickle.html) which will preserve the Qiskit object exactly but will only work for a single Qiskit version (it is also [potentially insecure](https://docs.python.org/3/library/pickle.html#module-pickle)).

## Using QPY

Using QPY is defined to be straightforward and mirror the user API of the serializers in Python’s standard library, `pickle` and `json`. There are 2 user facing functions: [`qiskit.qpy.dump()`](#qiskit.qpy.dump "qiskit.qpy.dump") and [`qiskit.qpy.load()`](#qiskit.qpy.load "qiskit.qpy.load") which are used to dump QPY data to a file object and load circuits from QPY data in a file object respectively. For example:

```python
from qiskit.circuit import QuantumCircuit
from qiskit import qpy

qc = QuantumCircuit(2, name='Bell', metadata={'test': True})
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

with open('bell.qpy', 'wb') as fd:
    qpy.dump(qc, fd)

with open('bell.qpy', 'rb') as fd:
    new_qc = qpy.load(fd)[0]
```

The [`qiskit.qpy.dump()`](#qiskit.qpy.dump "qiskit.qpy.dump") function also lets you include multiple circuits in a single QPY file:

```python
with open('twenty_bells.qpy', 'wb') as fd:
    qpy.dump([qc] * 20, fd)
```

and then loading that file will return a list with all the circuits

> **with open(‘twenty\_bells.qpy’, ‘rb’) as fd:**
>
> twenty\_new\_bells = qpy.load(fd)

### API documentation

<span id="qiskit.qpy.load" />

`qiskit.qpy.load(file_obj, metadata_deserializer=None)`

Load a QPY binary file

This function is used to load a serialized QPY Qiskit program file and create [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") objects or [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.schedule.ScheduleBlock") objects from its contents. For example:

```python
from qiskit import qpy

with open('bell.qpy', 'rb') as fd:
    circuits = qpy.load(fd)
```

or with a gzip compressed file:

```python
import gzip
from qiskit import qpy

with gzip.open('bell.qpy.gz', 'rb') as fd:
    circuits = qpy.load(fd)
```

which will read the contents of the qpy and return a list of [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") objects or [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.schedule.ScheduleBlock") objects from the file.

**Parameters**

*   **file\_obj** ([*BinaryIO*](https://docs.python.org/3/library/typing.html#typing.BinaryIO "(in Python v3.11)")) – A file like object that contains the QPY binary data for a circuit or pulse schedule.
*   **metadata\_deserializer** ([*Type*](https://docs.python.org/3/library/typing.html#typing.Type "(in Python v3.11)")*\[JSONDecoder] | None*) – An optional JSONDecoder class that will be used for the `cls` kwarg on the internal `json.load` call used to deserialize the JSON payload used for the `.metadata` attribute for any programs in the QPY file. If this is not specified the circuit metadata will be parsed as JSON with the stdlib `json.load()` function using the default `JSONDecoder` class.

**Returns**

The list of Qiskit programs contained in the QPY data. A list is always returned, even if there is only 1 program in the QPY data.

**Raises**

*   [**QiskitError**](exceptions#qiskit.exceptions.QiskitError "qiskit.exceptions.QiskitError") – if `file_obj` is not a valid QPY file
*   [**TypeError**](https://docs.python.org/3/library/exceptions.html#TypeError "(in Python v3.11)") – When invalid data type is loaded.

**Return type**

[*List*](https://docs.python.org/3/library/typing.html#typing.List "(in Python v3.11)")\[[*QuantumCircuit*](qiskit.circuit.QuantumCircuit "qiskit.circuit.quantumcircuit.QuantumCircuit") | [*ScheduleBlock*](qiskit.pulse.ScheduleBlock "qiskit.pulse.schedule.ScheduleBlock")]

<span id="qiskit.qpy.dump" />

`qiskit.qpy.dump(programs, file_obj, metadata_serializer=None)`

Write QPY binary data to a file

This function is used to save a circuit to a file for later use or transfer between machines. The QPY format is backwards compatible and can be loaded with future versions of Qiskit.

For example:

```python
from qiskit.circuit import QuantumCircuit
from qiskit import qpy

qc = QuantumCircuit(2, name='Bell', metadata={'test': True})
qc.h(0)
qc.cx(0, 1)
qc.measure_all()
```

from this you can write the qpy data to a file:

```python
with open('bell.qpy', 'wb') as fd:
    qpy.dump(qc, fd)
```

or a gzip compressed file:

```python
import gzip

with gzip.open('bell.qpy.gz', 'wb') as fd:
    qpy.dump(qc, fd)
```

Which will save the qpy serialized circuit to the provided file.

<Admonition title="Deprecated since version 0.21.0" type="danger">
  `qiskit.qpy.interface.dump()`’s argument `circuits` is deprecated as of qiskit-terra 0.21.0. It will be removed no earlier than 3 months after the release date. Instead, use the argument `programs`, which behaves identically.
</Admonition>

**Parameters**

*   **programs** ([*List*](https://docs.python.org/3/library/typing.html#typing.List "(in Python v3.11)")*\[*[*QuantumCircuit*](qiskit.circuit.QuantumCircuit "qiskit.circuit.quantumcircuit.QuantumCircuit")  *|*[*ScheduleBlock*](qiskit.pulse.ScheduleBlock "qiskit.pulse.schedule.ScheduleBlock")*] |* [*QuantumCircuit*](qiskit.circuit.QuantumCircuit "qiskit.circuit.quantumcircuit.QuantumCircuit")  *|*[*ScheduleBlock*](qiskit.pulse.ScheduleBlock "qiskit.pulse.schedule.ScheduleBlock")) – QPY supported object(s) to store in the specified file like object. QPY supports [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") and [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock"). Different data types must be separately serialized.
*   **file\_obj** ([*BinaryIO*](https://docs.python.org/3/library/typing.html#typing.BinaryIO "(in Python v3.11)")) – The file like object to write the QPY data too
*   **metadata\_serializer** ([*Type*](https://docs.python.org/3/library/typing.html#typing.Type "(in Python v3.11)")*\[JSONEncoder] | None*) – An optional JSONEncoder class that will be passed the `.metadata` attribute for each program in `programs` and will be used as the `cls` kwarg on the json.dump()\` call to JSON serialize that dictionary.

**Raises**

*   [**QpyError**](#qiskit.qpy.QpyError "qiskit.qpy.QpyError") – When multiple data format is mixed in the output.
*   [**TypeError**](https://docs.python.org/3/library/exceptions.html#TypeError "(in Python v3.11)") – When invalid data type is input.

These functions will raise a custom subclass of [`QiskitError`](exceptions#qiskit.exceptions.QiskitError "qiskit.exceptions.QiskitError") if they encounter problems during serialization or deserialization.

<span id="qiskit.qpy.QpyError" />

`qiskit.qpy.QpyError(*message)`

Errors raised by the qpy module.

Set the error message.

### QPY Compatibility

The QPY format is designed to be backwards compatible moving forward. This means you should be able to load a QPY with any newer Qiskit version than the one that generated it. However, loading a QPY file with an older Qiskit version is not supported and may not work.

For example, if you generated a QPY file using qiskit-terra 0.18.1 you could load that QPY file with qiskit-terra 0.19.0 and a hypothetical qiskit-terra 0.29.0. However, loading that QPY file with 0.18.0 is not supported and may not work.

<span id="id1" />

## QPY Format

The QPY serialization format is a portable cross-platform binary serialization format for [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") objects in Qiskit. The basic file format is as follows:

A QPY file (or memory object) always starts with the following 7 byte UTF8 string: `QISKIT` which is immediately followed by the overall file header. The contents of the file header as defined as a C struct are:

```python
struct {
    uint8_t qpy_version;
    uint8_t qiskit_major_version;
    uint8_t qiskit_minor_version;
    uint8_t qiskit_patch_version;
    uint64_t num_circuits;
}
```

All values use network byte order [\[1\]](#f1) (big endian) for cross platform compatibility.

The file header is immediately followed by the circuit payloads. Each individual circuit is composed of the following parts:

`HEADER | METADATA | REGISTERS | CUSTOM_DEFINITIONS | INSTRUCTIONS`

There is a circuit payload for each circuit (where the total number is dictated by `num_circuits` in the file header). There is no padding between the circuits in the data.

<span id="qpy-version-9" />

### Version 9

Version 9 addds support for classical [`Expr`](circuit_classical#qiskit.circuit.classical.expr.Expr "qiskit.circuit.classical.expr.Expr") nodes and their associated [`Type`](circuit_classical#qiskit.circuit.classical.types.Type "qiskit.circuit.classical.types.Type")s.

#### EXPRESSION

An [`Expr`](circuit_classical#qiskit.circuit.classical.expr.Expr "qiskit.circuit.classical.expr.Expr") node is represented by a stream of variable-width data. A node itself is represented by (in order in the byte stream):

1.  a one-byte type code discriminator;
2.  an EXPR\_TYPE object;
3.  a type-code-specific additional payload;
4.  a type-code-specific number of child EXPRESSION payloads (the number of these is implied by the type code and not explicitly stored).

Each of these are described in the following table:

| Qiskit class                                                                                              | Type code | Payload                                                                                                                                                              | Children |
| --------------------------------------------------------------------------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| [`Var`](circuit_classical#qiskit.circuit.classical.expr.Var "qiskit.circuit.classical.expr.Var")          | `x`       | One EXPR\_VAR.                                                                                                                                                       | 0        |
| [`Value`](circuit_classical#qiskit.circuit.classical.expr.Value "qiskit.circuit.classical.expr.Value")    | `v`       | One EXPR\_VALUE.                                                                                                                                                     | 0        |
| [`Cast`](circuit_classical#qiskit.circuit.classical.expr.Cast "qiskit.circuit.classical.expr.Cast")       | `c`       | One `_Bool` that corresponds to the value of `implicit`.                                                                                                             | 1        |
| [`Unary`](circuit_classical#qiskit.circuit.classical.expr.Unary "qiskit.circuit.classical.expr.Unary")    | `u`       | One `uint8_t` with the same numeric value as the [`Unary.Op`](circuit_classical#qiskit.circuit.classical.expr.Unary.Op "qiskit.circuit.classical.expr.Unary.Op").    | 1        |
| [`Binary`](circuit_classical#qiskit.circuit.classical.expr.Binary "qiskit.circuit.classical.expr.Binary") | `b`       | One `uint8_t` with the same numeric value as the [`Binary.Op`](circuit_classical#qiskit.circuit.classical.expr.Binary.Op "qiskit.circuit.classical.expr.Binary.Op"). | 2        |

<span id="expr-type" />

#### EXPR\_TYPE

A [`Type`](circuit_classical#qiskit.circuit.classical.types.Type "qiskit.circuit.classical.types.Type") is encoded by a single-byte ASCII `char` that encodes the kind of type, followed by a payload that varies depending on the type. The defined codes are:

| Qiskit class                                                                                          | Type code | Payload               |
| ----------------------------------------------------------------------------------------------------- | --------- | --------------------- |
| [`Bool`](circuit_classical#qiskit.circuit.classical.types.Bool "qiskit.circuit.classical.types.Bool") | `b`       | None.                 |
| [`Uint`](circuit_classical#qiskit.circuit.classical.types.Uint "qiskit.circuit.classical.types.Uint") | `u`       | One `uint32_t width`. |

<span id="expr-var" />

#### EXPR\_VAR

This represents a runtime variable of a [`Var`](circuit_classical#qiskit.circuit.classical.expr.Var "qiskit.circuit.classical.expr.Var") node. These are a type code, followed by a type-code-specific payload:

| Python class                                                                               | Type code | Payload                                                                                                                         |
| ------------------------------------------------------------------------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------- |
| [`Clbit`](qiskit.circuit.Clbit "qiskit.circuit.Clbit")                                     | `C`       | One `uint32_t index` that is the index of the [`Clbit`](qiskit.circuit.Clbit "qiskit.circuit.Clbit") in the containing circuit. |
| [`ClassicalRegister`](qiskit.circuit.ClassicalRegister "qiskit.circuit.ClassicalRegister") | `R`       | One `uint16_t reg_name_size`, followed by that many bytes of UTF-8 string data of the register name.                            |

<span id="expr-value" />

#### EXPR\_VALUE

This represents a literal object in the classical type system, such as an integer. Currently there are very few such literals. These are encoded as a type code, followed by a type-code-specific payload.

| Python type | Type code | Payload                                                                                                                                  |
| ----------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `bool`      | `b`       | One `_Bool value`.                                                                                                                       |
| `int`       | `i`       | One `uint8_t num_bytes`, followed by the integer encoded into that many many bytes (network order) in a two’s complement representation. |

#### Changes to INSTRUCTION

To support the use of [`Expr`](circuit_classical#qiskit.circuit.classical.expr.Expr "qiskit.circuit.classical.expr.Expr") nodes in the fields `IfElseOp.condition`, `WhileLoopOp.condition` and `SwitchCaseOp.target`, the INSTRUCTION struct is changed in an ABI compatible-manner to [its previous definition](#qpy-instruction-v5). The new struct is the C struct:

```python
struct {
    uint16_t name_size;
    uint16_t label_size;
    uint16_t num_parameters;
    uint32_t num_qargs;
    uint32_t num_cargs;
    uint8_t conditional_key;
    uint16_t conditional_reg_name_size;
    int64_t conditional_value;
    uint32_t num_ctrl_qubits;
    uint32_t ctrl_state;
}
```

where the only change is that a `uint8_t conditional_key` entry has replaced `_Bool has_conditional`. This new `conditional_key` takes the following numeric values, with these effects:

| Value | Effects                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | The instruction has its `.condition` field set to `None`. The `conditional_reg_name_size` and `conditional_value` fields should be ignored.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 1     | The instruction has its `.condition` field set to a 2-tuple of either a [`Clbit`](qiskit.circuit.Clbit "qiskit.circuit.Clbit") or a [`ClassicalRegister`](qiskit.circuit.ClassicalRegister "qiskit.circuit.ClassicalRegister"), and a integer of value `conditional_value`. The INSTRUCTION payload, including its trailing data is parsed exactly as it would be in QPY versions less than 8.                                                                                                                                                                                                                                        |
| 2     | The instruction has its `.condition` field set to a [`Expr`](circuit_classical#qiskit.circuit.classical.expr.Expr "qiskit.circuit.classical.expr.Expr") node. The `conditional_reg_name_size` and `conditional_value` fields should be ignored. The data following the struct is followed (as in QPY versions less than 8) by `name_size` bytes of UTF-8 string data for the class name and `label_size` bytes of UTF-8 string data for the label (if any). Then, there is one INSTRUCTION\_PARAM, which will contain an EXPRESSION. After that, parsing continues with the INSTRUCTION\_ARG structs, as in previous versions of QPY. |

<span id="changes-to-instruction-param" />

#### Changes to INSTRUCTION\_PARAM

A new type code `x` is added that defines an EXPRESSION parameter.

<span id="qpy-version-8" />

### Version 8

Version 8 adds support for handling a [`TranspileLayout`](qiskit.transpiler.TranspileLayout "qiskit.transpiler.TranspileLayout") stored in the [`QuantumCircuit.layout`](qiskit.circuit.QuantumCircuit#layout "qiskit.circuit.QuantumCircuit.layout") attribute. In version 8 immediately following the calibrations block at the end of the circuit payload there is now the `LAYOUT` struct. This struct outlines the size of the three attributes of a [`TranspileLayout`](qiskit.transpiler.TranspileLayout "qiskit.transpiler.TranspileLayout") class.

#### LAYOUT

```python
struct {
    char exists;
    int32_t initial_layout_size;
    int32_t input_mapping_size;
    int32_t final_layout_size;
    uint32_t extra_registers;
}
```

If any of the signed values are `-1` this indicates the corresponding attribute is `None`.

Immediately following the `LAYOUT` struct there is a [REGISTERS](#qpy-registers) struct for `extra_registers` (specifically the format introduced in [Version 4](#qpy-version-4)) standalone register definitions that aren’t present in the circuit. Then there are `initial_layout_size` `INITIAL_LAYOUT_BIT` structs to define the [`TranspileLayout.initial_layout`](qiskit.transpiler.TranspileLayout#initial_layout "qiskit.transpiler.TranspileLayout.initial_layout") attribute.

<span id="initial-layout-bit" />

#### INITIAL\_LAYOUT\_BIT

```python
struct {
    int32_t index;
    int32_t register_size;
}
```

Where a value of `-1` indicates `None` (as in no register is associated with the bit). Following each `INITIAL_LAYOUT_BIT` struct is `register_size` bytes for a `utf8` encoded string for the register name.

Following the initial layout there is `input_mapping_size` array of `uint32_t` integers representing the positions of the phyiscal bit from the initial layout. This enables constructing a list of virtual bits where the array index is its input mapping position.

Finally, there is an array of `final_layout_size` `uint32_t` integers. Each element is an index in the circuit’s `qubits` attribute which enables building a mapping from qubit starting position to the output position at the end of the circuit.

<span id="qpy-version-7" />

### Version 7

Version 7 adds support for `Reference` instruction and serialization of a `ScheduleBlock` program while keeping its reference to subroutines:

```python
from qiskit import pulse
from qiskit import qpy

with pulse.build() as schedule:
    pulse.reference("cr45p", "q0", "q1")
    pulse.reference("x", "q0")
    pulse.reference("cr45p", "q0", "q1")

with open('template_ecr.qpy', 'wb') as fd:
    qpy.dump(schedule, fd)
```

The conventional [SCHEDULE\_BLOCK](#qpy-schedule-block) data model is preserved, but in version 7 it is immediately followed by an extra [MAPPING](#qpy-mapping) utf8 bytes block representing the data of the referenced subroutines.

New type key character is added to the [SCHEDULE\_BLOCK\_INSTRUCTIONS](#qpy-schedule-instructions) group for the `Reference` instruction.

*   `y`: [`Reference`](qiskit.pulse.instructions.Reference "qiskit.pulse.instructions.Reference") instruction

New type key character is added to the [SCHEDULE\_BLOCK\_OPERANDS](#qpy-schedule-operands) group for the operands of `Reference` instruction, which is a tuple of strings, e.g. (“cr45p”, “q0”, “q1”).

*   `o`: string (operand string)

Note that this is the same encoding with the built-in Python string, however, the standard value encoding in QPY uses `s` type character for string data, which conflicts with the [`SymbolicPulse`](qiskit.pulse.library.SymbolicPulse "qiskit.pulse.library.SymbolicPulse") in the scope of pulse instruction operands. A special type character `o` is reserved for the string data that appears in the pulse instruction operands.

In addition, version 7 adds two new type keys to the INSTRUCTION\_PARM struct. `"d"` is followed by no data and represents the literal value [`CASE_DEFAULT`](circuit#qiskit.circuit.CASE_DEFAULT "qiskit.circuit.CASE_DEFAULT") for switch-statement support. `"R"` represents a [`ClassicalRegister`](qiskit.circuit.ClassicalRegister "qiskit.circuit.ClassicalRegister") or [`Clbit`](qiskit.circuit.Clbit "qiskit.circuit.Clbit"), and is followed by the same format as the description of register or classical bit as used in the first element of [the condition of an INSTRUCTION field](#qpy-instructions).

<span id="qpy-version-6" />

### Version 6

Version 6 adds support for `ScalableSymbolicPulse`. These objects are saved and read like SymbolicPulse objects, and the class name is added to the data to correctly handle the class selection.

SymbolicPulse block now starts with SYMBOLIC\_PULSE\_V2 header:

```python
struct {
    uint16_t class_name_size;
    uint16_t type_size;
    uint16_t envelope_size;
    uint16_t constraints_size;
    uint16_t valid_amp_conditions_size;
    _bool amp_limited;
}
```

The only change compared to [Version 5](#qpy-version-5) is the addition of class\_name\_size. The header is then immediately followed by `class_name_size` utf8 bytes with the name of the class. Currently, either SymbolicPulse or ScalableSymbolicPulse are supported. The rest of the data is then identical to [Version 5](#qpy-version-5).

<span id="qpy-version-5" />

### Version 5

Version 5 changes from [Version 4](#qpy-version-4) by adding support for `ScheduleBlock` and changing two payloads the INSTRUCTION metadata payload and the CUSTOM\_INSTRUCTION block. These now have new fields to better account for [`ControlledGate`](qiskit.circuit.ControlledGate "qiskit.circuit.ControlledGate") objects in a circuit. In addition, new payload MAP\_ITEM is defined to implement the [MAPPING](#qpy-mapping) block.

With the support of `ScheduleBlock`, now [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") can be serialized together with [`calibrations`](qiskit.circuit.QuantumCircuit#calibrations "qiskit.circuit.QuantumCircuit.calibrations"), or [Pulse Gates](https://qiskit.org/documentation/tutorials/circuits_advanced/05_pulse_gates.html). In QPY version 5 and above, [CIRCUIT\_CALIBRATIONS](#qpy-circuit-calibrations) payload is packed after the [INSTRUCTIONS](#qpy-instructions) block.

In QPY version 5 and above,

```python
struct {
    char type;
}
```

immediately follows the file header block to represent the program type stored in the file.

*   When `type==c`, [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") payload follows
*   When `type==s`, [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock") payload follows

<Admonition title="Note" type="note">
  Different programs cannot be packed together in the same file. You must create different files for different program types. Multiple objects with the same type can be saved in a single file.
</Admonition>

<span id="schedule-block" />

<span id="qpy-schedule-block" />

#### SCHEDULE\_BLOCK

[`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock") is first supported in QPY Version 5. This allows users to save pulse programs in the QPY binary format as follows:

```python
from qiskit import pulse, qpy

with pulse.build() as schedule:
    pulse.play(pulse.Gaussian(160, 0.1, 40), pulse.DriveChannel(0))

with open('schedule.qpy', 'wb') as fd:
    qpy.dump(qc, fd)

with open('schedule.qpy', 'rb') as fd:
    new_qc = qpy.load(fd)[0]
```

Note that circuit and schedule block are serialized and deserialized through the same QPY interface. Input data type is implicitly analyzed and no extra option is required to save the schedule block.

<span id="schedule-block-header" />

<span id="qpy-schedule-block-header" />

#### SCHEDULE\_BLOCK\_HEADER

[`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock") block starts with the following header:

```python
struct {
    uint16_t name_size;
    uint64_t metadata_size;
    uint16_t num_element;
}
```

which is immediately followed by `name_size` utf8 bytes of schedule name and `metadata_size` utf8 bytes of the JSON serialized metadata dictionary attached to the schedule.

<span id="schedule-block-alignments" />

<span id="qpy-schedule-alignments" />

#### SCHEDULE\_BLOCK\_ALIGNMENTS

Then, alignment context of the schedule block starts with `char` representing the supported context type followed by the [SEQUENCE](#qpy-sequence) block representing the parameters associated with the alignment context `AlignmentKind._context_params`. The context type char is mapped to each alignment subclass as follows:

*   `l`: [`AlignLeft`](qiskit.pulse.transforms.AlignLeft "qiskit.pulse.transforms.AlignLeft")
*   `r`: [`AlignRight`](qiskit.pulse.transforms.AlignRight "qiskit.pulse.transforms.AlignRight")
*   `s`: [`AlignSequential`](qiskit.pulse.transforms.AlignSequential "qiskit.pulse.transforms.AlignSequential")
*   `e`: [`AlignEquispaced`](qiskit.pulse.transforms.AlignEquispaced "qiskit.pulse.transforms.AlignEquispaced")

Note that [`AlignFunc`](qiskit.pulse.transforms.AlignFunc "qiskit.pulse.transforms.AlignFunc") context is not supported becasue of the callback function stored in the context parameters.

<span id="schedule-block-instructions" />

<span id="qpy-schedule-instructions" />

#### SCHEDULE\_BLOCK\_INSTRUCTIONS

This alignment block is further followed by `num_element` length of block elements which may consist of nested schedule blocks and schedule instructions. Each schedule instruction starts with `char` representing the instruction type followed by the [SEQUENCE](#qpy-sequence) block representing the instruction `operands`. Note that the data structure of pulse [`Instruction`](pulse#qiskit.pulse.instructions.Instruction "qiskit.pulse.instructions.Instruction") is unified so that instance can be uniquely determied by the class and a tuple of operands. The mapping of type char to the instruction subclass is defined as follows:

*   `a`: [`Acquire`](qiskit.pulse.instructions.Acquire "qiskit.pulse.instructions.Acquire") instruction
*   `p`: [`Play`](qiskit.pulse.instructions.Play "qiskit.pulse.instructions.Play") instruction
*   `d`: [`Delay`](qiskit.pulse.instructions.Delay "qiskit.pulse.instructions.Delay") instruction
*   `f`: [`SetFrequency`](qiskit.pulse.instructions.SetFrequency "qiskit.pulse.instructions.SetFrequency") instruction
*   `g`: [`ShiftFrequency`](qiskit.pulse.instructions.ShiftFrequency "qiskit.pulse.instructions.ShiftFrequency") instruction
*   `q`: [`SetPhase`](qiskit.pulse.instructions.SetPhase "qiskit.pulse.instructions.SetPhase") instruction
*   `r`: [`ShiftPhase`](qiskit.pulse.instructions.ShiftPhase "qiskit.pulse.instructions.ShiftPhase") instruction
*   `b`: [`RelativeBarrier`](qiskit.pulse.instructions.RelativeBarrier "qiskit.pulse.instructions.RelativeBarrier") instruction
*   `t`: [`TimeBlockade`](qiskit.pulse.instructions.TimeBlockade "qiskit.pulse.instructions.TimeBlockade") instruction
*   `y`: [`Reference`](qiskit.pulse.instructions.Reference "qiskit.pulse.instructions.Reference") instruction (new in version 0.7)

<span id="schedule-block-operands" />

<span id="qpy-schedule-operands" />

#### SCHEDULE\_BLOCK\_OPERANDS

The operands of these instances can be serialized through the standard QPY value serialization mechanism, however there are special object types that only appear in the schedule operands. Since the operands are serialized as [SEQUENCE](#qpy-sequence), each element must be packed with the INSTRUCTION\_PARAM pack struct, where each payload starts with a header block consists of the char `type` and uint64\_t `size`. Special objects start with the following type key:

*   `c`: [`Channel`](pulse#qiskit.pulse.channels.Channel "qiskit.pulse.channels.Channel")
*   `w`: [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform")
*   `s`: [`SymbolicPulse`](qiskit.pulse.library.SymbolicPulse "qiskit.pulse.library.SymbolicPulse")
*   `o`: string (operand string, new in version 0.7)

<span id="qpy-schedule-channel" />

#### CHANNEL

Channel block starts with channel subtype `char` that maps an object data to [`Channel`](pulse#qiskit.pulse.channels.Channel "qiskit.pulse.channels.Channel") subclass. Mapping is defined as follows:

*   `d`: [`DriveChannel`](qiskit.pulse.channels.DriveChannel "qiskit.pulse.channels.DriveChannel")
*   `c`: [`ControlChannel`](qiskit.pulse.channels.ControlChannel "qiskit.pulse.channels.ControlChannel")
*   `m`: [`MeasureChannel`](qiskit.pulse.channels.MeasureChannel "qiskit.pulse.channels.MeasureChannel")
*   `a`: [`AcquireChannel`](qiskit.pulse.channels.AcquireChannel "qiskit.pulse.channels.AcquireChannel")
*   `e`: [`MemorySlot`](qiskit.pulse.channels.MemorySlot "qiskit.pulse.channels.MemorySlot")
*   `r`: [`RegisterSlot`](qiskit.pulse.channels.RegisterSlot "qiskit.pulse.channels.RegisterSlot")

The key is immediately followed by the channel index serialized as the INSTRUCTION\_PARAM.

<span id="qpy-schedule-waveform" />

#### Waveform

Waveform block starts with WAVEFORM header:

```python
struct {
    double epsilon;
    uint32_t data_size;
    _bool amp_limited;
}
```

which is followed by `data_size` bytes of complex `ndarray` binary generated by [numpy.save](https://numpy.org/doc/stable/reference/generated/numpy.save.html). This represents the complex IQ data points played on a quantum device. [`name`](qiskit.pulse.library.Waveform#name "qiskit.pulse.library.Waveform.name") is saved after the samples in the INSTRUCTION\_PARAM pack struct, which can be string or `None`.

<span id="qpy-schedule-symbolic-pulse" />

#### SymbolicPulse

SymbolicPulse block starts with SYMBOLIC\_PULSE header:

```python
struct {
    uint16_t type_size;
    uint16_t envelope_size;
    uint16_t constraints_size;
    uint16_t valid_amp_conditions_size;
    _bool amp_limited;
}
```

which is followed by `type_size` utf8 bytes of [`SymbolicPulse.pulse_type`](qiskit.pulse.library.SymbolicPulse#pulse_type "qiskit.pulse.library.SymbolicPulse.pulse_type") string that represents a class of waveform, such as “Gaussian” or “GaussianSquare”. Then, `envelope_size`, `constraints_size`, `valid_amp_conditions_size` utf8 bytes of serialized symbolic expressions are generated for [`SymbolicPulse.envelope`](qiskit.pulse.library.SymbolicPulse#envelope "qiskit.pulse.library.SymbolicPulse.envelope"), [`SymbolicPulse.constraints`](qiskit.pulse.library.SymbolicPulse#constraints "qiskit.pulse.library.SymbolicPulse.constraints"), and [`SymbolicPulse.valid_amp_conditions`](qiskit.pulse.library.SymbolicPulse#valid_amp_conditions "qiskit.pulse.library.SymbolicPulse.valid_amp_conditions"), respectively. Since string representation of these expressions are usually lengthy, the expression binary is generated by the python [zlib](https://docs.python.org/3/library/zlib.html) module with data compression.

To uniquely specify a pulse instance, we also need to store the associated parameters, which consist of `duration` and the rest of parameters as a dictionary. Dictionary parameters are first dumped in the [MAPPING](#qpy-mapping) form, and then `duration` is dumped with the INSTRUCTION\_PARAM pack struct. Lastly, [`name`](qiskit.pulse.library.SymbolicPulse#name "qiskit.pulse.library.SymbolicPulse.name") is saved also with the INSTRUCTION\_PARAM pack struct, which can be string or `None`.

<span id="qpy-mapping" />

#### MAPPING

The MAPPING is a representation for arbitrary mapping object. This is a fixed length [SEQUENCE](#qpy-sequence) of key-value pair represented by the MAP\_ITEM payload.

A MAP\_ITEM starts with a header defined as:

```python
struct {
    uint16_t key_size;
    char type;
    uint16_t size;
}
```

which is immediately followed by the `key_size` utf8 bytes representing the dictionary key in string and `size` utf8 bytes of arbitrary object data of QPY serializable `type`.

<span id="circuit-calibrations" />

<span id="qpy-circuit-calibrations" />

#### CIRCUIT\_CALIBRATIONS

The CIRCUIT\_CALIBRATIONS block is a dictionary to define pulse calibrations of the custom instruction set. This block starts with the following CALIBRATION header:

```python
struct {
    uint16_t num_cals;
}
```

which is followed by the `num_cals` length of calibration entries, each starts with the CALIBRATION\_DEF header:

```python
struct {
    uint16_t name_size;
    uint16_t num_qubits;
    uint16_t num_params;
    char type;
}
```

The calibration definition header is then followed by `name_size` utf8 bytes of the gate name, `num_qubits` length of integers representing a sequence of qubits, and `num_params` length of INSTRUCTION\_PARAM payload for parameters associated to the custom instruction. The `type` indicates the class of pulse program which is either, in pricinple, [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock") or [`Schedule`](qiskit.pulse.Schedule "qiskit.pulse.Schedule"). As of QPY Version 5, only [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock") payload is supported. Finally, [SCHEDULE\_BLOCK](#qpy-schedule-block) payload is packed for each CALIBRATION\_DEF entry.

<span id="qpy-instruction-v5" />

#### INSTRUCTION

The INSTRUCTION block was modified to add two new fields `num_ctrl_qubits` and `ctrl_state` which are used to model the [`ControlledGate.num_ctrl_qubits`](qiskit.circuit.ControlledGate#num_ctrl_qubits "qiskit.circuit.ControlledGate.num_ctrl_qubits") and [`ControlledGate.ctrl_state`](qiskit.circuit.ControlledGate#ctrl_state "qiskit.circuit.ControlledGate.ctrl_state") attributes. The new payload packed struct format is:

```python
struct {
    uint16_t name_size;
    uint16_t label_size;
    uint16_t num_parameters;
    uint32_t num_qargs;
    uint32_t num_cargs;
    _Bool has_conditional;
    uint16_t conditional_reg_name_size;
    int64_t conditional_value;
    uint32_t num_ctrl_qubits;
    uint32_t ctrl_state;
}
```

The rest of the instruction payload is the same. You can refer to [INSTRUCTIONS](#qpy-instructions) for the details of the full payload.

<span id="custom-instruction" />

#### CUSTOM\_INSTRUCTION

The CUSTOM\_INSTRUCTION block in QPY version 5 adds a new field `base_gate_size` which is used to define the size of the [`qiskit.circuit.Instruction`](qiskit.circuit.Instruction "qiskit.circuit.Instruction") object stored in the `ControlledGate.base_gate` attribute for a custom [`ControlledGate`](qiskit.circuit.ControlledGate "qiskit.circuit.ControlledGate") object. With this change the CUSTOM\_INSTRUCTION metadata block becomes:

```python
struct {
    uint16_t name_size;
    char type;
    uint32_t num_qubits;
    uint32_t num_clbits;
    _Bool custom_definition;
    uint64_t size;
    uint32_t num_ctrl_qubits;
    uint32_t ctrl_state;
    uint64_t base_gate_size
}
```

Immediately following the CUSTOM\_INSTRUCTION struct is the utf8 encoded name of size `name_size`.

If `custom_definition` is `True` that means that the immediately following `size` bytes contains a QPY circuit data which can be used for the custom definition of that gate. If `custom_definition` is `False` then the instruction can be considered opaque (ie no definition). The `type` field determines what type of object will get created with the custom definition. If it’s `'g'` it will be a [`Gate`](qiskit.circuit.Gate "qiskit.circuit.Gate") object, `'i'` it will be a [`Instruction`](qiskit.circuit.Instruction "qiskit.circuit.Instruction") object.

Following this the next `base_gate_size` bytes contain the `INSTRUCTION` payload for the `ControlledGate.base_gate`.

Additionally an addition value for `type` is added `'c'` which is used to indicate the custom instruction is a custom [`ControlledGate`](qiskit.circuit.ControlledGate "qiskit.circuit.ControlledGate").

<span id="qpy-version-4" />

### Version 4

Version 4 is identical to [Version 3](#qpy-version-3) except that it adds 2 new type strings to the INSTRUCTION\_PARAM struct, `z` to represent `None` (which is encoded as no data), `q` to represent a [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") (which is encoded as a QPY circuit), `r` to represent a `range` of integers (which is encoded as a [RANGE](#qpy-range-pack)), and `t` to represent a `sequence` (which is encoded as defined by [SEQUENCE](#qpy-sequence)). Additionally, version 4 changes the type of register index mapping array from `uint32_t` to `int64_t`. If the values of any of the array elements are negative they represent a register bit that is not present in the circuit.

The [REGISTERS](#qpy-registers) header format has also been updated to

```python
struct {
    char type;
    _Bool standalone;
    uint32_t size;
    uint16_t name_size;
    _bool in_circuit;
}
```

which just adds the `in_circuit` field which represents whether the register is part of the circuit or not.

<span id="qpy-range-pack" />

#### RANGE

A RANGE is a representation of a `range` object. It is defined as:

```python
struct {
    int64_t start;
    int64_t stop;
    int64_t step;
}
```

<span id="qpy-sequence" />

#### SEQUENCE

A SEQUENCE is a reprentation of a arbitrary sequence object. As sequence are just fixed length containers of arbitrary python objects their QPY can’t fully represent any sequence, but as long as the contents in a sequence are other QPY serializable types for the INSTRUCTION\_PARAM payload the `sequence` object can be serialized.

A sequence instruction parameter starts with a header defined as:

```python
struct {
    uint64_t size;
}
```

followed by `size` elements that are INSTRUCTION\_PARAM payloads, where each of these define an element in the sequence. The sequence object will be typecasted into proper type, e.g. `tuple`, afterwards.

<span id="qpy-version-3" />

### Version 3

Version 3 of the QPY format is identical to [Version 2](#qpy-version-2) except that it defines a struct format to represent a [`PauliEvolutionGate`](qiskit.circuit.library.PauliEvolutionGate "qiskit.circuit.library.PauliEvolutionGate") natively in QPY. To accomplish this the [CUSTOM\_DEFINITIONS](#qpy-custom-definition) struct now supports a new type value `'p'` to represent a [`PauliEvolutionGate`](qiskit.circuit.library.PauliEvolutionGate "qiskit.circuit.library.PauliEvolutionGate"). Enties in the custom instructions tables have unique name generated that start with the string `"###PauliEvolutionGate_"` followed by a uuid string. This gate name is reservered in QPY and if you have a custom [`Instruction`](qiskit.circuit.Instruction "qiskit.circuit.Instruction") object with a definition set and that name prefix it will error. If it’s of type `'p'` the data payload is defined as follows:

<span id="pauli-evolution" />

<span id="pauli-evo-qpy" />

#### PAULI\_EVOLUTION

This represents the high level [`PauliEvolutionGate`](qiskit.circuit.library.PauliEvolutionGate "qiskit.circuit.library.PauliEvolutionGate")

```python
struct {
    uint64_t operator_count;
    _Bool standalone_op;
    char time_type;
    uint64_t time_size;
    uint64_t synthesis_size;
}
```

This is immediately followed by `operator_count` elements defined by the [SPARSE\_PAULI\_OP\_LIST\_ELEM](#qpy-pauli-sum-op) payload. Following that we have `time_size` bytes representing the `time` attribute. If `standalone_op` is `True` then there must only be a single operator. The encoding of these bytes is determined by the value of `time_type`. Possible values of `time_type` are `'f'`, `'p'`, and `'e'`. If `time_type` is `'f'` it’s a double, `'p'` defines a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") object which is represented by a [PARAMETER](#qpy-param-struct), `e` defines a [`ParameterExpression`](qiskit.circuit.ParameterExpression "qiskit.circuit.ParameterExpression") object (that’s not a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter")) which is represented by a [PARAMETER\_EXPR](#qpy-param-expr). Following that is `synthesis_size` bytes which is a utf8 encoded json payload representing the [`EvolutionSynthesis`](qiskit.synthesis.EvolutionSynthesis "qiskit.synthesis.EvolutionSynthesis") class used by the gate.

<span id="sparse-pauli-op-list-elem" />

<span id="qpy-pauli-sum-op" />

#### SPARSE\_PAULI\_OP\_LIST\_ELEM

This represents an instance of [`PauliSumOp`](http://qiskit.org/documentation/stubs/qiskit.opflow.primitive_ops.PauliSumOp.html#qiskit.opflow.primitive_ops.PauliSumOp "qiskit.opflow.primitive_ops.PauliSumOp").

```python
struct {
    uint32_t pauli_op_size;
}
```

which is immediately followed by `pauli_op_size` bytes which are .npy format [\[2\]](#f2) data which represents the [`SparsePauliOp`](qiskit.quantum_info.SparsePauliOp "qiskit.quantum_info.SparsePauliOp").

Version 3 of the QPY format also defines a struct format to represent a `ParameterVectorElement` as a distinct subclass from a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter"). This adds a new parameter type char `'v'` to represent a `ParameterVectorElement` which is now supported as a type string value for an INSTRUCTION\_PARAM. The payload for these parameters are defined below as [PARAMETER\_VECTOR\_ELEMENT](#qpy-param-vector).

<span id="parameter-vector-element" />

<span id="qpy-param-vector" />

#### PARAMETER\_VECTOR\_ELEMENT

A PARAMETER\_VECTOR\_ELEMENT represents a `ParameterVectorElement` object the data for a INSTRUCTION\_PARAM. The contents of the PARAMETER\_VECTOR\_ELEMENT are defined as:

```python
struct {
    uint16_t vector_name_size;
    uint64_t vector_size;
    char uuid[16];
    uint64_t index;
}
```

which is immediately followed by `vector_name_size` utf8 bytes representing the parameter’s vector name.

<span id="parameter-expr" />

<span id="qpy-param-expr-v3" />

#### PARAMETER\_EXPR

Additionally, since QPY format version v3 distinguishes between a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") and `ParameterVectorElement` the payload for a [`ParameterExpression`](qiskit.circuit.ParameterExpression "qiskit.circuit.ParameterExpression") needs to be updated to distinguish between the types. The following is the modified payload format which is mostly identical to the format in Version 1 and [Version 2](#qpy-version-2) but just modifies the `map_elements` struct to include a symbol type field.

A PARAMETER\_EXPR represents a [`ParameterExpression`](qiskit.circuit.ParameterExpression "qiskit.circuit.ParameterExpression") object that the data for an INSTRUCTION\_PARAM. The contents of a PARAMETER\_EXPR are defined as:

```python
struct {
    uint64_t map_elements;
    uint64_t expr_size;
}
```

Immediately following the header is `expr_size` bytes of utf8 data containing the expression string, which is the sympy srepr of the expression for the parameter expression. Following that is a symbol map which contains `map_elements` elements with the format

```python
struct {
    char symbol_type;
    char type;
    uint64_t size;
}
```

The `symbol_type` key determines the payload type of the symbol representation for the element. If it’s `p` it represents a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") and if it’s `v` it represents a `ParameterVectorElement`. The map element struct is immediately followed by the symbol map key payload, if `symbol_type` is `p` then it is followed immediately by a [PARAMETER](#qpy-param-struct) object (both the struct and utf8 name bytes) and if `symbol_type` is `v` then the struct is imediately followed by [PARAMETER\_VECTOR\_ELEMENT](#qpy-param-vector) (both the struct and utf8 name bytes). That is followed by `size` bytes for the data of the symbol. The data format is dependent on the value of `type`. If `type` is `p` then it represents a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") and size will be 0, the value will just be the same as the key. Similarly if the `type` is `v` then it represents a `ParameterVectorElement` and size will be 0 as the value will just be the same as the key. If `type` is `f` then it represents a double precision float. If `type` is `c` it represents a double precision complex, which is represented by the [COMPLEX](#qpy-complex). Finally, if type is `i` it represents an integer which is an `int64_t`.

<span id="qpy-version-2" />

### Version 2

Version 2 of the QPY format is identical to version 1 except for the HEADER section is slightly different. You can refer to the [Version 1](#qpy-version-1) section for the details on the rest of the payload format.

#### HEADER

The contents of HEADER are defined as a C struct are:

```python
struct {
    uint16_t name_size;
    char global_phase_type;
    uint16_t global_phase_size;
    uint32_t num_qubits;
    uint32_t num_clbits;
    uint64_t metadata_size;
    uint32_t num_registers;
    uint64_t num_instructions;
    uint64_t num_custom_gates;
}
```

This is immediately followed by `name_size` bytes of utf8 data for the name of the circuit. Following this is immediately `global_phase_size` bytes representing the global phase. The content of that data is dictated by the value of `global_phase_type`. If it’s `'f'` the data is a float and is the size of a `double`. If it’s `'p'` defines a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") object which is represented by a PARAM struct (see below), `e` defines a [`ParameterExpression`](qiskit.circuit.ParameterExpression "qiskit.circuit.ParameterExpression") object (that’s not a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter")) which is represented by a PARAM\_EXPR struct (see below).

<span id="qpy-version-1" />

### Version 1

<span id="id4" />

#### HEADER

The contents of HEADER as defined as a C struct are:

```python
struct {
    uint16_t name_size;
    double global_phase;
    uint32_t num_qubits;
    uint32_t num_clbits;
    uint64_t metadata_size;
    uint32_t num_registers;
    uint64_t num_instructions;
    uint64_t num_custom_gates;
}
```

This is immediately followed by `name_size` bytes of utf8 data for the name of the circuit.

#### METADATA

The METADATA field is a UTF8 encoded JSON string. After reading the HEADER (which is a fixed size at the start of the QPY file) and the `name` string you then read the `metadata_size` number of bytes and parse the JSON to get the metadata for the circuit.

<span id="qpy-registers" />

#### REGISTERS

The contents of REGISTERS is a number of REGISTER object. If num\_registers is > 0 then after reading METADATA you read that number of REGISTER structs defined as:

```python
struct {
    char type;
    _Bool standalone;
    uint32_t size;
    uint16_t name_size;
}
```

`type` can be `'q'` or `'c'`.

Immediately following the REGISTER struct is the utf8 encoded register name of size `name_size`. After the `name` utf8 bytes there is then an array of int64\_t values of size `size` that contains a map of the register’s index to the circuit’s qubit index. For example, array element 0’s value is the index of the `register[0]`’s position in the containing circuit’s qubits list.

<Admonition title="Note" type="note">
  Prior to QPY [Version 4](#qpy-version-4) the type of array elements was uint32\_t. This was changed to enable negative values which represent bits in the array not present in the circuit
</Admonition>

The standalone boolean determines whether the register is constructed as a standalone register that was added to the circuit or was created from existing bits. A register is considered standalone if it has bits constructed solely as part of it, for example:

```python
qr = QuantumRegister(2)
qc = QuantumCircuit(qr)
```

the register `qr` would be a standalone register. While something like:

```python
bits = [Qubit(), Qubit()]
qr2 = QuantumRegister(bits=bits)
qc = QuantumCircuit(qr2)
```

`qr2` would have `standalone` set to `False`.

<span id="custom-definitions" />

<span id="qpy-custom-definition" />

#### CUSTOM\_DEFINITIONS

This section specifies custom definitions for any of the instructions in the circuit.

CUSTOM\_DEFINITION\_HEADER contents are defined as:

```python
struct {
    uint64_t size;
}
```

If size is greater than 0 that means the circuit contains custom instruction(s). Each custom instruction is defined with a CUSTOM\_INSTRUCTION block defined as:

```python
struct {
    uint16_t name_size;
    char type;
    uint32_t num_qubits;
    uint32_t num_clbits;
    _Bool custom_definition;
    uint64_t size;
}
```

Immediately following the CUSTOM\_INSTRUCTION struct is the utf8 encoded name of size `name_size`.

If `custom_definition` is `True` that means that the immediately following `size` bytes contains a QPY circuit data which can be used for the custom definition of that gate. If `custom_definition` is `False` then the instruction can be considered opaque (ie no definition). The `type` field determines what type of object will get created with the custom definition. If it’s `'g'` it will be a [`Gate`](qiskit.circuit.Gate "qiskit.circuit.Gate") object, `'i'` it will be a [`Instruction`](qiskit.circuit.Instruction "qiskit.circuit.Instruction") object.

<span id="qpy-instructions" />

#### INSTRUCTIONS

The contents of INSTRUCTIONS is a list of INSTRUCTION metadata objects

```python
struct {
    uint16_t name_size;
    uint16_t label_size;
    uint16_t num_parameters;
    uint32_t num_qargs;
    uint32_t num_cargs;
    _Bool has_conditional;
    uint16_t conditional_reg_name_size;
    int64_t conditional_value;
}
```

This metadata object is immediately followed by `name_size` bytes of utf8 bytes for the `name`. `name` here is the Qiskit class name for the Instruction class if it’s defined in Qiskit. Otherwise it falls back to the custom instruction name. Following the `name` bytes there are `label_size` bytes of utf8 data for the label if one was set on the instruction. Following the label bytes if `has_conditional` is `True` then there are `conditional_reg_name_size` bytes of utf8 data for the name of the conditional register name. In case of single classical bit conditions the register name utf8 data will be prefixed with a null character “x00” and then a utf8 string integer representing the classical bit index in the circuit that the condition is on.

This is immediately followed by the INSTRUCTION\_ARG structs for the list of arguments of that instruction. These are in the order of all quantum arguments (there are num\_qargs of these) followed by all classical arguments (num\_cargs of these).

The contents of each INSTRUCTION\_ARG is:

```python
struct {
    char type;
    uint32_t index;
}
```

`type` can be `'q'` or `'c'`.

After all arguments for an instruction the parameters are specified with `num_parameters` INSTRUCTION\_PARAM structs.

The contents of each INSTRUCTION\_PARAM is:

```python
struct {
    char type;
    uint64_t size;
}
```

After each INSTRUCTION\_PARAM the next `size` bytes are the parameter’s data. The `type` field can be `'i'`, `'f'`, `'p'`, `'e'`, `'s'`, `'c'` or `'n'` which dictate the format. For `'i'` it’s an integer, `'f'` it’s a double, `'s'` if it’s a string (encoded as utf8), `'c'` is a complex and the data is represented by the struct format in the [PARAMETER\_EXPR](#qpy-param-expr) section. `'p'` defines a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") object which is represented by a [PARAMETER](#qpy-param-struct) struct, `e` defines a [`ParameterExpression`](qiskit.circuit.ParameterExpression "qiskit.circuit.ParameterExpression") object (that’s not a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter")) which is represented by a [PARAMETER\_EXPR](#qpy-param-expr) struct (on QPY format [Version 3](#qpy-version-3) the format is tweak slightly see: [PARAMETER\_EXPR](#qpy-param-expr-v3)), `'n'` represents an object from numpy (either an `ndarray` or a numpy type) which means the data is .npy format [\[2\]](#f2) data, and in QPY [Version 3](#qpy-version-3) `'v'` represents a `ParameterVectorElement` which is represented by a [PARAMETER\_VECTOR\_ELEMENT](#qpy-param-vector) struct.

<span id="qpy-param-struct" />

#### PARAMETER

A PARAMETER represents a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") object the data for a INSTRUCTION\_PARAM. The contents of the PARAMETER are defined as:

```python
struct {
    uint16_t name_size;
    char uuid[16];
}
```

which is immediately followed by `name_size` utf8 bytes representing the parameter name.

<span id="qpy-param-expr" />

<span id="id6" />

#### PARAMETER\_EXPR

A PARAMETER\_EXPR represents a [`ParameterExpression`](qiskit.circuit.ParameterExpression "qiskit.circuit.ParameterExpression") object that the data for an INSTRUCTION\_PARAM. The contents of a PARAMETER\_EXPR are defined as:

The PARAMETER\_EXPR data starts with a header:

```python
struct {
    uint64_t map_elements;
    uint64_t expr_size;
}
```

Immediately following the header is `expr_size` bytes of utf8 data containing the expression string, which is the sympy srepr of the expression for the parameter expression. Follwing that is a symbol map which contains `map_elements` elements with the format

```python
struct {
    char type;
    uint64_t size;
}
```

Which is followed immediately by `PARAMETER` object (both the struct and utf8 name bytes) for the symbol map key. That is followed by `size` bytes for the data of the symbol. The data format is dependent on the value of `type`. If `type` is `p` then it represents a [`Parameter`](qiskit.circuit.Parameter "qiskit.circuit.Parameter") and size will be 0, the value will just be the same as the key. If `type` is `f` then it represents a double precision float. If `type` is `c` it represents a double precision complex, which is represented by [COMPLEX](#qpy-complex). Finally, if type is `i` it represents an integer which is an `int64_t`.

<span id="qpy-complex" />

#### COMPLEX

When representing a double precision complex value in QPY the following struct is used:

```python
struct {
    double real;
    double imag;
}
```

this matches the internal C representation of Python’s complex type. [\[3\]](#f3)

\[[1](#id2)]

[https://tools.ietf.org/html/rfc1700](https://tools.ietf.org/html/rfc1700)

\[2] ([1](#id3),[2](#id5))

[https://numpy.org/doc/stable/reference/generated/numpy.lib.format.html](https://numpy.org/doc/stable/reference/generated/numpy.lib.format.html)

\[[3](#id7)]

[https://docs.python.org/3/c-api/complex.html#c.Py\_complex](https://docs.python.org/3/c-api/complex.html#c.Py_complex)
