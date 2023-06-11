```python
@Cd = chramp("ramp", chf("val"));
```

[Expression functions](https://www.sidefx.com/docs/houdini/expressions/index.html).
[houdini vex function](https://www.sidefx.com/docs/houdini/vex/functions/index.html).

# chramp
_Evaluates a ramp parameter and return its value._


```python
float  chramp(string channel, float ramppos) // x value is input, y value is result.

float  chramp(string channel, float ramppos, float time)

vector  chramp(string channel, float ramppos)

vector  chramp(string channel, float ramppos, float time)
```

Evaluates a ramp parameter and return its value.

The ramppos is where on the ramp to evaluate. The ramppos is clamped to the range [0,1].

The time parameter can be used if the ramp is animated to evaluate at other than the current time.

# Parameter expressions
_You can enter expressions into a parameter so its value is computed instead of being static or keyframe animated._
