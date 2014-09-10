---
---

### TangoFX
TangoFX is a Scala / JavaFX wrapper for the [TangORB](http://www.tango-controls.org/download/tangorb) library. It provides complete ObjectProperty[T] implementation representing a generic device attribute. Convenient wrappers for TangORB API proxies are also available.

[![Build Status](https://travis-ci.org/mliszcz/tangofx.svg?branch=master)](https://travis-ci.org/mliszcz/tangofx)

### Features
 * ObjectProperty[T] compatible with any device attribute
 * wrappers for DeviceProxy, AttributeProxy and command interface
 * ScalaFX-like binding operators (`<==`, `==>`, `<=>`)
 * `caseOf` conditional binding (matching value / boolean expression)
 * implicit conversion from lambdas to `ChangeListener`, `InvalidationListener` and `EventHandler` objects
 * unsigned short type
 * *(soon) new controls*

### Demo
``` scala

val lblName = new Label
val nfValue = new NumberField
val btnExec = new Button

val dev = new TangoDevice("tango://172.16.181.133:10000/k2/mod/01")
val att = dev.getAttribute[Double]("SolenoidPs1Voltage")
val cmd = dev.getCommand[String, Unit]("ModStateRead")

lblName.textProperty     <== att.name
nfValue.valueProperty    <=> att.value

nfValue.readOnlyProperty <== (caseOf
    when (att.readOnly)                   take    true
    when (att.value greaterThan 100.0)    take    true
    default false
end)

att.value.addListener( (newValue: Double) => println(s"new value $newvalue") )
btnExec.setOnAction( (e: ActionEvent) => println("status: " + cmd.execute()) )
```

### Adding TangoFX to your project

Build the project:

```
$ git clone https://github.com/mliszcz/tangofx.git
$ cd tangofx
$ sbt update publish-local
```

Then add tangofx dependency:

```
io.github.tangofx % tangofx_2.11 % 0.0.1-SNAPSHOT
```
