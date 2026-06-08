# Architecture

Captures the integrated system architecture for the platform.

## System overview

_Block diagram and one-paragraph description of the platform: modules, base
station, host computer, recording-system interfaces._

## Communication protocol

_Selected protocol for the module network (e.g. CAN bus). Justification, link
to ADR._

## CAN bus topology

_Topology, addressing scheme, connector strategy, cable assumptions, max node
count, terminator placement._

## Module hardware

_MCU, motor driver, pellet sensing, interaction sensing, LED/status hardware._

## Base station hardware

_Selected base station platform and rationale._

## Synchronization strategy

_High-level decision: modules vs base station vs both. Detailed sync I/O
defined in [`sync-and-recording.md`](sync-and-recording.md)._

## Status indicators

_LED states: boot, fault, active, reward, etc. Cross-reference to
[`failure-modes.md`](failure-modes.md) for fault codes._

## Power

_Power source, distribution, current budget._

## Architecture diagram

_Insert or link to diagram._

## Open questions

_Track unresolved architectural questions until they become ADRs._
