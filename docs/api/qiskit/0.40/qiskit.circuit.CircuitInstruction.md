---
title: CircuitInstruction
description: API reference for qiskit.circuit.CircuitInstruction
in_page_toc_min_heading_level: 1
python_api_type: class
python_api_name: qiskit.circuit.CircuitInstruction
---

# CircuitInstruction[¶](#circuitinstruction "Permalink to this headline")

<span id="qiskit.circuit.CircuitInstruction" />

`CircuitInstruction(operation, qubits=(), clbits=())`

Bases: `object`

A single instruction in a [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit"), comprised of the [`operation`](#qiskit.circuit.CircuitInstruction.operation "qiskit.circuit.CircuitInstruction.operation") and various operands.

<Admonition title="Warning" type="caution">
  This is a lightweight internal class and there is minimal error checking; you must respect the type hints when using it. It is the user’s responsibility to ensure that direct mutations of the object do not invalidate the types, nor the restrictions placed on it by its context. Typically this will mean, for example, that [`qubits`](#qiskit.circuit.CircuitInstruction.qubits "qiskit.circuit.CircuitInstruction.qubits") must be a sequence of distinct items, with no duplicates.
</Admonition>

## Methods

### copy

<span id="qiskit.circuit.CircuitInstruction.copy" />

`CircuitInstruction.copy()`

Return a shallow copy of the [`CircuitInstruction`](qiskit.circuit.CircuitInstruction "qiskit.circuit.CircuitInstruction").

**Return type**

[`CircuitInstruction`](qiskit.circuit.CircuitInstruction "qiskit.circuit.quantumcircuitdata.CircuitInstruction")

### replace

<span id="qiskit.circuit.CircuitInstruction.replace" />

`CircuitInstruction.replace(operation=None, qubits=None, clbits=None)`

Return a new [`CircuitInstruction`](qiskit.circuit.CircuitInstruction "qiskit.circuit.CircuitInstruction") with the given fields replaced.

**Return type**

[`CircuitInstruction`](qiskit.circuit.CircuitInstruction "qiskit.circuit.quantumcircuitdata.CircuitInstruction")

## Attributes

<span id="qiskit.circuit.CircuitInstruction.operation" />

### operation

`qiskit.circuit.instruction.Instruction`

The logical operation that this instruction represents an execution of.

<span id="qiskit.circuit.CircuitInstruction.qubits" />

### qubits

`Tuple[qiskit.circuit.quantumregister.Qubit, ...]`

A sequence of the qubits that the operation is applied to.

<span id="qiskit.circuit.CircuitInstruction.clbits" />

### clbits

`Tuple[qiskit.circuit.classicalregister.Clbit, ...]`

A sequence of the classical bits that this operation reads from or writes to.
