# Infinite Flight Connect API Docs & Samples
## Sample apps

---

- [Offical demo](https://github.com/mlaban/IFCTest/tree/master/Infinite%20Flight%20Connector%20API)
- [Liveflight Connect](https://github.com/LiveFlightApp/Connect-Windows)
- [Demo - v2](https://github.com/carmichaelalonso/infiniteflightapi)

## Connect API - Version 2.0

---

This version of the API was first included in Infinite Flight version 19.4.

## Connection

---

1. To enable Infinite Flight command server, check `Enable Infinite Flight Connect` in `Settings > General`
2. Infinite Flight will broadcast UDP packets on port `15000` containing its own IP address and Port, as well as other information. Example message: `["State": Playing, "Port": 10111, "DeviceID": iPad7, "Aircraft": Cessna 172, "Version": 19.4.7354.25209, "DeviceName": Thomas’s iPad, "Addresses":("fe80::1c79:baf4:f9f1:dd59%3", "192.168.1.26"), "Livery": Civil Air Patrol]`
3. You must then establish a TCP connection on this given host and port (note that version 2 uses port `10112`, not port `10111` -- that's from v1).

## Interacting with the API

---

### Obtaining the Manifest

The Connect API exposes different states for every aircraft, and as such the applicable states and their IDs change depending on what aircraft you are operating. You can obtain a manifest of available states by sending integer `-1` followed by boolean `false` to the API. This will return the `ID` you sent (`-1`), and the `length` of the data you are about to receive in bytes as an integer. All information received from the API begins this way. Following the `ID` and `length`, the manifest is represented as a (very long) string: The ConnectAPI represents strings by sending their length in bytes as an integer (yes, again), followed by the actual string. The string will have the format: `511,2,aircraft/0/systems/nav_sources/adf/2/distance_to_glide_path\n851,3,infiniteflight/cameras/4/x_angle\n81,4,api_joystick/buttons/8/name...`. This string can be split on `\n` and then generalizes to `ID, Type, Path` for each state.

The API defines data types according to the following integers:

| Integer | Type             |
| ------- | ---------------- |
| 0       | Boolean          |
| 1       | Integer (32-bit) |
| 2       | Float            |
| 3       | Double           |
| 4       | String           |
| 5       | Long             |

### Sending Data

Sending data is done by sending information to the API in one of the following ways:

#### GetState

A `GetState` command is sent by sending:

1. The integer value of the `ID` of the state you would like to get.
2. A `false` boolean value.

The state you request will be returned via the API on the same socket. 

E.g. sending `635`, `false` will result in Infinite Flight sending you `635`,`1`,`1` if your strobe lights are on.

#### SetState

A `SetState` command is sent by sending:

1. The integer value of the `ID` of the state you would like to get.
2. A `true` boolean value.
3. The value, as the applicable data type, you would like to set the state to. 

E.g. send `635`, `true`, `0` will turn your strobe lights off.

#### RunCommand

A `RunCommand` is sent by sending:

1. The integer value of the `ID` of the command you would like to send.
2. A `false` boolean value

E.g. sending `1048634`, `false` will toggle the autopilot.

### Receiving Data

All states begin with two integer values:  `ID` and `Length` of the data.

Following these values, all states are returned as their data type (so a state with an integer data type will be returned as an integer, a float as a float, etc.) with the exception of strings. Strings contain one additional integer specifying the length of bytes as an integer, followed by the string.

You can receive states after sending a `GetState` command, described above.

## Simulator States

---

### Cessna 172 (Steam)


ID|Type|Path|Description
---|---|---|---
0|1|api_joystick/axes/count|
1|4|api_joystick/axes/0/name|
2|1|api_joystick/axes/0/value|
3|4|api_joystick/axes/1/name|
4|1|api_joystick/axes/1/value|
5|4|api_joystick/axes/2/name|
6|1|api_joystick/axes/2/value|
7|4|api_joystick/axes/3/name|
8|1|api_joystick/axes/3/value|
9|4|api_joystick/axes/4/name|
10|1|api_joystick/axes/4/value|
11|4|api_joystick/axes/5/name|
12|1|api_joystick/axes/5/value|
13|4|api_joystick/axes/6/name|
14|1|api_joystick/axes/6/value|
15|4|api_joystick/axes/7/name|
16|1|api_joystick/axes/7/value|
17|4|api_joystick/axes/8/name|
18|1|api_joystick/axes/8/value|
19|4|api_joystick/axes/9/name|
20|1|api_joystick/axes/9/value|
21|4|api_joystick/axes/10/name|
22|1|api_joystick/axes/10/value|
23|4|api_joystick/axes/11/name|
24|1|api_joystick/axes/11/value|
25|4|api_joystick/axes/12/name|
26|1|api_joystick/axes/12/value|
27|4|api_joystick/axes/13/name|
28|1|api_joystick/axes/13/value|
29|4|api_joystick/axes/14/name|
30|1|api_joystick/axes/14/value|
31|4|api_joystick/axes/15/name|
32|1|api_joystick/axes/15/value|
33|4|api_joystick/axes/16/name|
34|1|api_joystick/axes/16/value|
35|4|api_joystick/axes/17/name|
36|1|api_joystick/axes/17/value|
37|4|api_joystick/axes/18/name|
38|1|api_joystick/axes/18/value|
39|4|api_joystick/axes/19/name|
40|1|api_joystick/axes/19/value|
41|4|api_joystick/axes/20/name|
42|1|api_joystick/axes/20/value|
43|4|api_joystick/axes/21/name|
44|1|api_joystick/axes/21/value|
45|4|api_joystick/axes/22/name|
46|1|api_joystick/axes/22/value|
47|4|api_joystick/axes/23/name|
48|1|api_joystick/axes/23/value|
49|4|api_joystick/axes/24/name|
50|1|api_joystick/axes/24/value|
51|4|api_joystick/axes/25/name|
52|1|api_joystick/axes/25/value|
53|4|api_joystick/axes/26/name|
54|1|api_joystick/axes/26/value|
55|4|api_joystick/axes/27/name|
56|1|api_joystick/axes/27/value|
57|4|api_joystick/axes/28/name|
58|1|api_joystick/axes/28/value|
59|4|api_joystick/axes/29/name|
60|1|api_joystick/axes/29/value|
61|4|api_joystick/axes/30/name|
62|1|api_joystick/axes/30/value|
63|4|api_joystick/axes/31/name|
64|1|api_joystick/axes/31/value|
65|4|api_joystick/buttons/0/name|
66|1|api_joystick/buttons/0/value|
67|4|api_joystick/buttons/1/name|
68|1|api_joystick/buttons/1/value|
69|4|api_joystick/buttons/2/name|
70|1|api_joystick/buttons/2/value|
71|4|api_joystick/buttons/3/name|
72|1|api_joystick/buttons/3/value|
73|4|api_joystick/buttons/4/name|
74|1|api_joystick/buttons/4/value|
75|4|api_joystick/buttons/5/name|
76|1|api_joystick/buttons/5/value|
77|4|api_joystick/buttons/6/name|
78|1|api_joystick/buttons/6/value|
79|4|api_joystick/buttons/7/name|
80|1|api_joystick/buttons/7/value|
81|4|api_joystick/buttons/8/name|
82|1|api_joystick/buttons/8/value|
83|4|api_joystick/buttons/9/name|
84|1|api_joystick/buttons/9/value|
85|4|api_joystick/buttons/10/name|
86|1|api_joystick/buttons/10/value|
87|4|api_joystick/buttons/11/name|
88|1|api_joystick/buttons/11/value|
89|4|api_joystick/buttons/12/name|
90|1|api_joystick/buttons/12/value|
91|4|api_joystick/buttons/13/name|
92|1|api_joystick/buttons/13/value|
93|4|api_joystick/buttons/14/name|
94|1|api_joystick/buttons/14/value|
95|4|api_joystick/buttons/15/name|
96|1|api_joystick/buttons/15/value|
97|4|api_joystick/buttons/16/name|
98|1|api_joystick/buttons/16/value|
99|4|api_joystick/buttons/17/name|
100|1|api_joystick/buttons/17/value|
101|4|api_joystick/buttons/18/name|
102|1|api_joystick/buttons/18/value|
103|4|api_joystick/buttons/19/name|
104|1|api_joystick/buttons/19/value|
105|4|api_joystick/buttons/20/name|
106|1|api_joystick/buttons/20/value|
107|4|api_joystick/buttons/21/name|
108|1|api_joystick/buttons/21/value|
109|4|api_joystick/buttons/22/name|
110|1|api_joystick/buttons/22/value|
111|4|api_joystick/buttons/23/name|
112|1|api_joystick/buttons/23/value|
113|4|api_joystick/buttons/24/name|
114|1|api_joystick/buttons/24/value|
115|4|api_joystick/buttons/25/name|
116|1|api_joystick/buttons/25/value|
117|4|api_joystick/buttons/26/name|
118|1|api_joystick/buttons/26/value|
119|4|api_joystick/buttons/27/name|
120|1|api_joystick/buttons/27/value|
121|4|api_joystick/buttons/28/name|
122|1|api_joystick/buttons/28/value|
123|4|api_joystick/buttons/29/name|
124|1|api_joystick/buttons/29/value|
125|4|api_joystick/buttons/30/name|
126|1|api_joystick/buttons/30/value|
127|4|api_joystick/buttons/31/name|
128|1|api_joystick/buttons/31/value|
129|4|api_joystick/buttons/32/name|
130|1|api_joystick/buttons/32/value|
131|4|api_joystick/buttons/33/name|
132|1|api_joystick/buttons/33/value|
133|4|api_joystick/buttons/34/name|
134|1|api_joystick/buttons/34/value|
135|4|api_joystick/buttons/35/name|
136|1|api_joystick/buttons/35/value|
137|4|api_joystick/buttons/36/name|
138|1|api_joystick/buttons/36/value|
139|4|api_joystick/buttons/37/name|
140|1|api_joystick/buttons/37/value|
141|4|api_joystick/buttons/38/name|
142|1|api_joystick/buttons/38/value|
143|4|api_joystick/buttons/39/name|
144|1|api_joystick/buttons/39/value|
145|4|api_joystick/buttons/40/name|
146|1|api_joystick/buttons/40/value|
147|4|api_joystick/buttons/41/name|
148|1|api_joystick/buttons/41/value|
149|4|api_joystick/buttons/42/name|
150|1|api_joystick/buttons/42/value|
151|4|api_joystick/buttons/43/name|
152|1|api_joystick/buttons/43/value|
153|4|api_joystick/buttons/44/name|
154|1|api_joystick/buttons/44/value|
155|4|api_joystick/buttons/45/name|
156|1|api_joystick/buttons/45/value|
157|4|api_joystick/buttons/46/name|
158|1|api_joystick/buttons/46/value|
159|4|api_joystick/buttons/47/name|
160|1|api_joystick/buttons/47/value|
161|4|api_joystick/buttons/48/name|
162|1|api_joystick/buttons/48/value|
163|4|api_joystick/buttons/49/name|
164|1|api_joystick/buttons/49/value|
165|4|api_joystick/buttons/50/name|
166|1|api_joystick/buttons/50/value|
167|4|api_joystick/buttons/51/name|
168|1|api_joystick/buttons/51/value|
169|4|api_joystick/buttons/52/name|
170|1|api_joystick/buttons/52/value|
171|4|api_joystick/buttons/53/name|
172|1|api_joystick/buttons/53/value|
173|4|api_joystick/buttons/54/name|
174|1|api_joystick/buttons/54/value|
175|4|api_joystick/buttons/55/name|
176|1|api_joystick/buttons/55/value|
177|4|api_joystick/buttons/56/name|
178|1|api_joystick/buttons/56/value|
179|4|api_joystick/buttons/57/name|
180|1|api_joystick/buttons/57/value|
181|4|api_joystick/buttons/58/name|
182|1|api_joystick/buttons/58/value|
183|4|api_joystick/buttons/59/name|
184|1|api_joystick/buttons/59/value|
185|4|api_joystick/buttons/60/name|
186|1|api_joystick/buttons/60/value|
187|4|api_joystick/buttons/61/name|
188|1|api_joystick/buttons/61/value|
189|4|api_joystick/buttons/62/name|
190|1|api_joystick/buttons/62/value|
191|4|api_joystick/buttons/63/name|
192|1|api_joystick/buttons/63/value|
193|4|api_joystick/buttons/64/name|
194|1|api_joystick/buttons/64/value|
195|4|api_joystick/buttons/65/name|
196|1|api_joystick/buttons/65/value|
197|4|api_joystick/buttons/66/name|
198|1|api_joystick/buttons/66/value|
199|4|api_joystick/buttons/67/name|
200|1|api_joystick/buttons/67/value|
201|4|api_joystick/buttons/68/name|
202|1|api_joystick/buttons/68/value|
203|4|api_joystick/buttons/69/name|
204|1|api_joystick/buttons/69/value|
205|4|api_joystick/buttons/70/name|
206|1|api_joystick/buttons/70/value|
207|4|api_joystick/buttons/71/name|
208|1|api_joystick/buttons/71/value|
209|4|api_joystick/buttons/72/name|
210|1|api_joystick/buttons/72/value|
211|4|api_joystick/buttons/73/name|
212|1|api_joystick/buttons/73/value|
213|4|api_joystick/buttons/74/name|
214|1|api_joystick/buttons/74/value|
215|4|api_joystick/buttons/75/name|
216|1|api_joystick/buttons/75/value|
217|4|api_joystick/buttons/76/name|
218|1|api_joystick/buttons/76/value|
219|4|api_joystick/buttons/77/name|
220|1|api_joystick/buttons/77/value|
221|4|api_joystick/buttons/78/name|
222|1|api_joystick/buttons/78/value|
223|4|api_joystick/buttons/79/name|
224|1|api_joystick/buttons/79/value|
225|4|api_joystick/buttons/80/name|
226|1|api_joystick/buttons/80/value|
227|4|api_joystick/buttons/81/name|
228|1|api_joystick/buttons/81/value|
229|4|api_joystick/buttons/82/name|
230|1|api_joystick/buttons/82/value|
231|4|api_joystick/buttons/83/name|
232|1|api_joystick/buttons/83/value|
233|4|api_joystick/buttons/84/name|
234|1|api_joystick/buttons/84/value|
235|4|api_joystick/buttons/85/name|
236|1|api_joystick/buttons/85/value|
237|4|api_joystick/buttons/86/name|
238|1|api_joystick/buttons/86/value|
239|4|api_joystick/buttons/87/name|
240|1|api_joystick/buttons/87/value|
241|4|api_joystick/buttons/88/name|
242|1|api_joystick/buttons/88/value|
243|4|api_joystick/buttons/89/name|
244|1|api_joystick/buttons/89/value|
245|4|api_joystick/buttons/90/name|
246|1|api_joystick/buttons/90/value|
247|4|api_joystick/buttons/91/name|
248|1|api_joystick/buttons/91/value|
249|4|api_joystick/buttons/92/name|
250|1|api_joystick/buttons/92/value|
251|4|api_joystick/buttons/93/name|
252|1|api_joystick/buttons/93/value|
253|4|api_joystick/buttons/94/name|
254|1|api_joystick/buttons/94/value|
255|4|api_joystick/buttons/95/name|
256|1|api_joystick/buttons/95/value|
257|4|api_joystick/buttons/96/name|
258|1|api_joystick/buttons/96/value|
259|4|api_joystick/buttons/97/name|
260|1|api_joystick/buttons/97/value|
261|4|api_joystick/buttons/98/name|
262|1|api_joystick/buttons/98/value|
263|4|api_joystick/buttons/99/name|
264|1|api_joystick/buttons/99/value|
265|4|api_joystick/buttons/100/name|
266|1|api_joystick/buttons/100/value|
267|4|api_joystick/buttons/101/name|
268|1|api_joystick/buttons/101/value|
269|4|api_joystick/buttons/102/name|
270|1|api_joystick/buttons/102/value|
271|4|api_joystick/buttons/103/name|
272|1|api_joystick/buttons/103/value|
273|4|api_joystick/buttons/104/name|
274|1|api_joystick/buttons/104/value|
275|4|api_joystick/buttons/105/name|
276|1|api_joystick/buttons/105/value|
277|4|api_joystick/buttons/106/name|
278|1|api_joystick/buttons/106/value|
279|4|api_joystick/buttons/107/name|
280|1|api_joystick/buttons/107/value|
281|4|api_joystick/buttons/108/name|
282|1|api_joystick/buttons/108/value|
283|4|api_joystick/buttons/109/name|
284|1|api_joystick/buttons/109/value|
285|4|api_joystick/buttons/110/name|
286|1|api_joystick/buttons/110/value|
287|4|api_joystick/buttons/111/name|
288|1|api_joystick/buttons/111/value|
289|4|api_joystick/buttons/112/name|
290|1|api_joystick/buttons/112/value|
291|4|api_joystick/buttons/113/name|
292|1|api_joystick/buttons/113/value|
293|4|api_joystick/buttons/114/name|
294|1|api_joystick/buttons/114/value|
295|4|api_joystick/buttons/115/name|
296|1|api_joystick/buttons/115/value|
297|4|api_joystick/buttons/116/name|
298|1|api_joystick/buttons/116/value|
299|4|api_joystick/buttons/117/name|
300|1|api_joystick/buttons/117/value|
301|4|api_joystick/buttons/118/name|
302|1|api_joystick/buttons/118/value|
303|4|api_joystick/buttons/119/name|
304|1|api_joystick/buttons/119/value|
305|4|api_joystick/buttons/120/name|
306|1|api_joystick/buttons/120/value|
307|4|api_joystick/buttons/121/name|
308|1|api_joystick/buttons/121/value|
309|4|api_joystick/buttons/122/name|
310|1|api_joystick/buttons/122/value|
311|4|api_joystick/buttons/123/name|
312|1|api_joystick/buttons/123/value|
313|4|api_joystick/buttons/124/name|
314|1|api_joystick/buttons/124/value|
315|4|api_joystick/buttons/125/name|
316|1|api_joystick/buttons/125/value|
317|4|api_joystick/buttons/126/name|
318|1|api_joystick/buttons/126/value|
319|4|api_joystick/buttons/127/name|
320|1|api_joystick/buttons/127/value|
321|2|api_joystick/pov/x|
322|2|api_joystick/pov/y|
323|4|infiniteflight/api_version|
324|4|infiniteflight/app_version|
325|1|infiniteflight/display_width|
326|1|infiniteflight/display_height|
327|4|infiniteflight/app_state|
328|4|infiniteflight/play_mode|
329|4|infiniteflight/current_user|
330|2|environment/wind_direction_true|
331|2|environment/wind_velocity|
332|3|simulator/flight_time|
333|5|simulator/time_local|
334|5|simulator/time_utc|
335|4|simulator/time_zone|
336|0|atmosphere/isday|
337|2|atmosphere/speed_of_sound|
338|2|atmosphere/sea_level_air_density|
339|2|atmosphere/air_density|
340|4|infiniteflight/nearest_airports|
341|1|simulator/throttle|
342|2|aircraft/0/systems/engines/0/rpm|
343|2|aircraft/0/systems/engines/0/mixture_lever|
344|2|aircraft/0/systems/engines/0/propeller_lever|
345|2|aircraft/0/systems/engines/0/oil_temperature|
346|2|aircraft/0/systems/engines/0/oil_pressure|
347|2|aircraft/0/systems/engines/0/fuel_flow|
348|2|aircraft/0/systems/engines/0/egt|
349|2|aircraft/0/systems/engines/0/vacuum|
350|1|aircraft/0/systems/engines/0/fuel_pump/state|
351|1|aircraft/0/systems/engines/0/starter/state|
352|1|aircraft/0/systems/selected_nav_source|
353|1|aircraft/0/systems/bearings/0/source|
354|1|aircraft/0/systems/bearings/1/source|
355|0|aircraft/0/systems/nav_sources/nav/1/tuned|
356|0|aircraft/0/systems/nav_sources/nav/1/is_course_to|
357|3|aircraft/0/systems/nav_sources/nav/1/location/latitude|
358|3|aircraft/0/systems/nav_sources/nav/1/location/longitude|
359|3|aircraft/0/systems/nav_sources/nav/1/location/altitude|
360|3|aircraft/0/systems/nav_sources/nav/1/distance|
361|4|aircraft/0/systems/nav_sources/nav/1/name|
362|4|aircraft/0/systems/nav_sources/nav/1/fullname|
363|1|aircraft/0/systems/nav_sources/nav/1/frequency|
364|2|aircraft/0/systems/nav_sources/nav/1/course|
365|2|aircraft/0/systems/nav_sources/nav/1/target_course|
366|2|aircraft/0/systems/nav_sources/nav/1/bearing|
367|2|aircraft/0/systems/nav_sources/nav/1/xtrack_distance|
368|2|aircraft/0/systems/nav_sources/nav/1/bearing_course_difference|
369|1|aircraft/0/systems/nav_sources/nav/1/to_from_indicator_state|
370|2|aircraft/0/systems/nav_sources/nav/1/relative_bearing|
371|2|aircraft/0/systems/nav_sources/nav/1/glideslope_angle|
372|2|aircraft/0/systems/nav_sources/nav/1/horizontal_deflection|
373|2|aircraft/0/systems/nav_sources/nav/1/vertical_deflection|
374|3|aircraft/0/systems/nav_sources/nav/1/horizontal_deflection_ratio|
375|3|aircraft/0/systems/nav_sources/nav/1/vertical_deflection_ratio|
376|2|aircraft/0/systems/nav_sources/nav/1/distance_to_glide_path|
377|2|aircraft/0/systems/nav_sources/nav/1/distance_to_landing_path|
378|2|aircraft/0/systems/nav_sources/nav/1/distance_to_localizer|
379|0|aircraft/0/systems/nav_sources/nav/1/in_range|
380|0|aircraft/0/systems/nav_sources/nav/1/has_glideslope|
381|0|aircraft/0/systems/nav_sources/nav/1/has_localizer|
382|0|aircraft/0/systems/nav_sources/nav/2/tuned|
383|0|aircraft/0/systems/nav_sources/nav/2/is_course_to|
384|3|aircraft/0/systems/nav_sources/nav/2/location/latitude|
385|3|aircraft/0/systems/nav_sources/nav/2/location/longitude|
386|3|aircraft/0/systems/nav_sources/nav/2/location/altitude|
387|3|aircraft/0/systems/nav_sources/nav/2/distance|
388|4|aircraft/0/systems/nav_sources/nav/2/name|
389|4|aircraft/0/systems/nav_sources/nav/2/fullname|
390|1|aircraft/0/systems/nav_sources/nav/2/frequency|
391|2|aircraft/0/systems/nav_sources/nav/2/course|
392|2|aircraft/0/systems/nav_sources/nav/2/target_course|
393|2|aircraft/0/systems/nav_sources/nav/2/bearing|
394|2|aircraft/0/systems/nav_sources/nav/2/xtrack_distance|
395|2|aircraft/0/systems/nav_sources/nav/2/bearing_course_difference|
396|1|aircraft/0/systems/nav_sources/nav/2/to_from_indicator_state|
397|2|aircraft/0/systems/nav_sources/nav/2/relative_bearing|
398|2|aircraft/0/systems/nav_sources/nav/2/glideslope_angle|
399|2|aircraft/0/systems/nav_sources/nav/2/horizontal_deflection|
400|2|aircraft/0/systems/nav_sources/nav/2/vertical_deflection|
401|3|aircraft/0/systems/nav_sources/nav/2/horizontal_deflection_ratio|
402|3|aircraft/0/systems/nav_sources/nav/2/vertical_deflection_ratio|
403|2|aircraft/0/systems/nav_sources/nav/2/distance_to_glide_path|
404|2|aircraft/0/systems/nav_sources/nav/2/distance_to_landing_path|
405|2|aircraft/0/systems/nav_sources/nav/2/distance_to_localizer|
406|0|aircraft/0/systems/nav_sources/nav/2/in_range|
407|0|aircraft/0/systems/nav_sources/nav/2/has_glideslope|
408|0|aircraft/0/systems/nav_sources/nav/2/has_localizer|
409|0|aircraft/0/systems/nav_sources/nav/3/tuned|
410|0|aircraft/0/systems/nav_sources/nav/3/is_course_to|
411|3|aircraft/0/systems/nav_sources/nav/3/location/latitude|
412|3|aircraft/0/systems/nav_sources/nav/3/location/longitude|
413|3|aircraft/0/systems/nav_sources/nav/3/location/altitude|
414|3|aircraft/0/systems/nav_sources/nav/3/distance|
415|4|aircraft/0/systems/nav_sources/nav/3/name|
416|4|aircraft/0/systems/nav_sources/nav/3/fullname|
417|1|aircraft/0/systems/nav_sources/nav/3/frequency|
418|2|aircraft/0/systems/nav_sources/nav/3/course|
419|2|aircraft/0/systems/nav_sources/nav/3/target_course|
420|2|aircraft/0/systems/nav_sources/nav/3/bearing|
421|2|aircraft/0/systems/nav_sources/nav/3/xtrack_distance|
422|2|aircraft/0/systems/nav_sources/nav/3/bearing_course_difference|
423|1|aircraft/0/systems/nav_sources/nav/3/to_from_indicator_state|
424|2|aircraft/0/systems/nav_sources/nav/3/relative_bearing|
425|2|aircraft/0/systems/nav_sources/nav/3/glideslope_angle|
426|2|aircraft/0/systems/nav_sources/nav/3/horizontal_deflection|
427|2|aircraft/0/systems/nav_sources/nav/3/vertical_deflection|
428|3|aircraft/0/systems/nav_sources/nav/3/horizontal_deflection_ratio|
429|3|aircraft/0/systems/nav_sources/nav/3/vertical_deflection_ratio|
430|2|aircraft/0/systems/nav_sources/nav/3/distance_to_glide_path|
431|2|aircraft/0/systems/nav_sources/nav/3/distance_to_landing_path|
432|2|aircraft/0/systems/nav_sources/nav/3/distance_to_localizer|
433|0|aircraft/0/systems/nav_sources/nav/3/in_range|
434|0|aircraft/0/systems/nav_sources/nav/3/has_glideslope|
435|0|aircraft/0/systems/nav_sources/nav/3/has_localizer|
436|0|aircraft/0/systems/nav_sources/nav/4/tuned|
437|0|aircraft/0/systems/nav_sources/nav/4/is_course_to|
438|3|aircraft/0/systems/nav_sources/nav/4/location/latitude|
439|3|aircraft/0/systems/nav_sources/nav/4/location/longitude|
440|3|aircraft/0/systems/nav_sources/nav/4/location/altitude|
441|3|aircraft/0/systems/nav_sources/nav/4/distance|
442|4|aircraft/0/systems/nav_sources/nav/4/name|
443|4|aircraft/0/systems/nav_sources/nav/4/fullname|
444|1|aircraft/0/systems/nav_sources/nav/4/frequency|
445|2|aircraft/0/systems/nav_sources/nav/4/course|
446|2|aircraft/0/systems/nav_sources/nav/4/target_course|
447|2|aircraft/0/systems/nav_sources/nav/4/bearing|
448|2|aircraft/0/systems/nav_sources/nav/4/xtrack_distance|
449|2|aircraft/0/systems/nav_sources/nav/4/bearing_course_difference|
450|1|aircraft/0/systems/nav_sources/nav/4/to_from_indicator_state|
451|2|aircraft/0/systems/nav_sources/nav/4/relative_bearing|
452|2|aircraft/0/systems/nav_sources/nav/4/glideslope_angle|
453|2|aircraft/0/systems/nav_sources/nav/4/horizontal_deflection|
454|2|aircraft/0/systems/nav_sources/nav/4/vertical_deflection|
455|3|aircraft/0/systems/nav_sources/nav/4/horizontal_deflection_ratio|
456|3|aircraft/0/systems/nav_sources/nav/4/vertical_deflection_ratio|
457|2|aircraft/0/systems/nav_sources/nav/4/distance_to_glide_path|
458|2|aircraft/0/systems/nav_sources/nav/4/distance_to_landing_path|
459|2|aircraft/0/systems/nav_sources/nav/4/distance_to_localizer|
460|0|aircraft/0/systems/nav_sources/nav/4/in_range|
461|0|aircraft/0/systems/nav_sources/nav/4/has_glideslope|
462|0|aircraft/0/systems/nav_sources/nav/4/has_localizer|
463|0|aircraft/0/systems/nav_sources/adf/1/tuned|
464|0|aircraft/0/systems/nav_sources/adf/1/is_course_to|
465|3|aircraft/0/systems/nav_sources/adf/1/location/latitude|
466|3|aircraft/0/systems/nav_sources/adf/1/location/longitude|
467|3|aircraft/0/systems/nav_sources/adf/1/location/altitude|
468|3|aircraft/0/systems/nav_sources/adf/1/distance|
469|4|aircraft/0/systems/nav_sources/adf/1/name|
470|4|aircraft/0/systems/nav_sources/adf/1/fullname|
471|1|aircraft/0/systems/nav_sources/adf/1/frequency|
472|2|aircraft/0/systems/nav_sources/adf/1/course|
473|2|aircraft/0/systems/nav_sources/adf/1/target_course|
474|2|aircraft/0/systems/nav_sources/adf/1/bearing|
475|2|aircraft/0/systems/nav_sources/adf/1/xtrack_distance|
476|2|aircraft/0/systems/nav_sources/adf/1/bearing_course_difference|
477|1|aircraft/0/systems/nav_sources/adf/1/to_from_indicator_state|
478|2|aircraft/0/systems/nav_sources/adf/1/relative_bearing|
479|2|aircraft/0/systems/nav_sources/adf/1/glideslope_angle|
480|2|aircraft/0/systems/nav_sources/adf/1/horizontal_deflection|
481|2|aircraft/0/systems/nav_sources/adf/1/vertical_deflection|
482|3|aircraft/0/systems/nav_sources/adf/1/horizontal_deflection_ratio|
483|3|aircraft/0/systems/nav_sources/adf/1/vertical_deflection_ratio|
484|2|aircraft/0/systems/nav_sources/adf/1/distance_to_glide_path|
485|2|aircraft/0/systems/nav_sources/adf/1/distance_to_landing_path|
486|2|aircraft/0/systems/nav_sources/adf/1/distance_to_localizer|
487|0|aircraft/0/systems/nav_sources/adf/1/in_range|
488|0|aircraft/0/systems/nav_sources/adf/1/has_glideslope|
489|0|aircraft/0/systems/nav_sources/adf/1/has_localizer|
490|0|aircraft/0/systems/nav_sources/adf/2/tuned|
491|0|aircraft/0/systems/nav_sources/adf/2/is_course_to|
492|3|aircraft/0/systems/nav_sources/adf/2/location/latitude|
493|3|aircraft/0/systems/nav_sources/adf/2/location/longitude|
494|3|aircraft/0/systems/nav_sources/adf/2/location/altitude|
495|3|aircraft/0/systems/nav_sources/adf/2/distance|
496|4|aircraft/0/systems/nav_sources/adf/2/name|
497|4|aircraft/0/systems/nav_sources/adf/2/fullname|
498|1|aircraft/0/systems/nav_sources/adf/2/frequency|
499|2|aircraft/0/systems/nav_sources/adf/2/course|
500|2|aircraft/0/systems/nav_sources/adf/2/target_course|
501|2|aircraft/0/systems/nav_sources/adf/2/bearing|
502|2|aircraft/0/systems/nav_sources/adf/2/xtrack_distance|
503|2|aircraft/0/systems/nav_sources/adf/2/bearing_course_difference|
504|1|aircraft/0/systems/nav_sources/adf/2/to_from_indicator_state|
505|2|aircraft/0/systems/nav_sources/adf/2/relative_bearing|
506|2|aircraft/0/systems/nav_sources/adf/2/glideslope_angle|
507|2|aircraft/0/systems/nav_sources/adf/2/horizontal_deflection|
508|2|aircraft/0/systems/nav_sources/adf/2/vertical_deflection|
509|3|aircraft/0/systems/nav_sources/adf/2/horizontal_deflection_ratio|
510|3|aircraft/0/systems/nav_sources/adf/2/vertical_deflection_ratio|
511|2|aircraft/0/systems/nav_sources/adf/2/distance_to_glide_path|
512|2|aircraft/0/systems/nav_sources/adf/2/distance_to_landing_path|
513|2|aircraft/0/systems/nav_sources/adf/2/distance_to_localizer|
514|0|aircraft/0/systems/nav_sources/adf/2/in_range|
515|0|aircraft/0/systems/nav_sources/adf/2/has_glideslope|
516|0|aircraft/0/systems/nav_sources/adf/2/has_localizer|
517|2|aircraft/0/systems/battery/main_battery/voltage|
518|2|aircraft/0/systems/battery/main_battery/amp_hour|
519|2|aircraft/0/systems/battery/main_battery/amp_draw|
520|2|aircraft/0/systems/alternator/alternator/voltage|
521|2|aircraft/0/systems/alternator/alternator/amp_hour|
522|2|aircraft/0/systems/alternator/alternator/amp_draw|
523|2|aircraft/0/systems/battery/standby_battery/voltage|
524|2|aircraft/0/systems/battery/standby_battery/amp_hour|
525|2|aircraft/0/systems/battery/standby_battery/amp_draw|
526|4|aircraft/0/systems/electrical_switch/stdby_battery_switch/inputs/0|
527|1|aircraft/0/systems/electrical_switch/stdby_battery_switch/state|
528|2|aircraft/0/systems/electrical_switch/stdby_battery_switch/voltage|
529|2|aircraft/0/systems/electrical_switch/stdby_battery_switch/amp_draw|
530|4|aircraft/0/systems/electrical_breaker/stdby_battery_breaker/inputs/0|
531|1|aircraft/0/systems/electrical_breaker/stdby_battery_breaker/state|
532|2|aircraft/0/systems/electrical_breaker/stdby_battery_breaker/voltage|
533|2|aircraft/0/systems/electrical_breaker/stdby_battery_breaker/amp_draw|
534|4|aircraft/0/systems/electrical_switch/master_switch/inputs/0|
535|1|aircraft/0/systems/electrical_switch/master_switch/state|
536|2|aircraft/0/systems/electrical_switch/master_switch/voltage|
537|2|aircraft/0/systems/electrical_switch/master_switch/amp_draw|
538|4|aircraft/0/systems/electrical_switch/alternator_switch/inputs/0|
539|1|aircraft/0/systems/electrical_switch/alternator_switch/state|
540|2|aircraft/0/systems/electrical_switch/alternator_switch/voltage|
541|2|aircraft/0/systems/electrical_switch/alternator_switch/amp_draw|
542|4|aircraft/0/systems/electrical_breaker/main_breaker/inputs/0|
543|1|aircraft/0/systems/electrical_breaker/main_breaker/state|
544|2|aircraft/0/systems/electrical_breaker/main_breaker/voltage|
545|2|aircraft/0/systems/electrical_breaker/main_breaker/amp_draw|
546|4|aircraft/0/systems/electrical_breaker/alternator_breaker/inputs/0|
547|1|aircraft/0/systems/electrical_breaker/alternator_breaker/state|
548|2|aircraft/0/systems/electrical_breaker/alternator_breaker/voltage|
549|2|aircraft/0/systems/electrical_breaker/alternator_breaker/amp_draw|
550|4|aircraft/0/systems/electrical_bus/crossfeed_bus/inputs/0|
551|4|aircraft/0/systems/electrical_bus/crossfeed_bus/inputs/1|
552|2|aircraft/0/systems/electrical_bus/crossfeed_bus/voltage|
553|2|aircraft/0/systems/electrical_bus/crossfeed_bus/amp_draw|
554|4|aircraft/0/systems/electrical_breaker/warn_breaker/inputs/0|
555|1|aircraft/0/systems/electrical_breaker/warn_breaker/state|
556|2|aircraft/0/systems/electrical_breaker/warn_breaker/voltage|
557|2|aircraft/0/systems/electrical_breaker/warn_breaker/amp_draw|
558|4|aircraft/0/systems/electrical_bus/electrical_bus_1/inputs/0|
559|2|aircraft/0/systems/electrical_bus/electrical_bus_1/voltage|
560|2|aircraft/0/systems/electrical_bus/electrical_bus_1/amp_draw|
561|4|aircraft/0/systems/electrical_bus/electrical_bus_2/inputs/0|
562|2|aircraft/0/systems/electrical_bus/electrical_bus_2/voltage|
563|2|aircraft/0/systems/electrical_bus/electrical_bus_2/amp_draw|
564|4|aircraft/0/systems/electrical_breaker/avionics_bus_1_breaker/inputs/0|
565|1|aircraft/0/systems/electrical_breaker/avionics_bus_1_breaker/state|
566|2|aircraft/0/systems/electrical_breaker/avionics_bus_1_breaker/voltage|
567|2|aircraft/0/systems/electrical_breaker/avionics_bus_1_breaker/amp_draw|
568|4|aircraft/0/systems/electrical_breaker/avionics_bus_2_breaker/inputs/0|
569|1|aircraft/0/systems/electrical_breaker/avionics_bus_2_breaker/state|
570|2|aircraft/0/systems/electrical_breaker/avionics_bus_2_breaker/voltage|
571|2|aircraft/0/systems/electrical_breaker/avionics_bus_2_breaker/amp_draw|
572|4|aircraft/0/systems/electrical_switch/avionics_bus_1_switch/inputs/0|
573|1|aircraft/0/systems/electrical_switch/avionics_bus_1_switch/state|
574|2|aircraft/0/systems/electrical_switch/avionics_bus_1_switch/voltage|
575|2|aircraft/0/systems/electrical_switch/avionics_bus_1_switch/amp_draw|
576|4|aircraft/0/systems/electrical_switch/avionics_bus_2_switch/inputs/0|
577|1|aircraft/0/systems/electrical_switch/avionics_bus_2_switch/state|
578|2|aircraft/0/systems/electrical_switch/avionics_bus_2_switch/voltage|
579|2|aircraft/0/systems/electrical_switch/avionics_bus_2_switch/amp_draw|
580|4|aircraft/0/systems/electrical_bus/avionics_bus_1/inputs/0|
581|2|aircraft/0/systems/electrical_bus/avionics_bus_1/voltage|
582|2|aircraft/0/systems/electrical_bus/avionics_bus_1/amp_draw|
583|4|aircraft/0/systems/electrical_bus/avionics_bus_2/inputs/0|
584|2|aircraft/0/systems/electrical_bus/avionics_bus_2/voltage|
585|2|aircraft/0/systems/electrical_bus/avionics_bus_2/amp_draw|
586|4|aircraft/0/systems/electrical_bus/essential_bus/inputs/0|
587|4|aircraft/0/systems/electrical_bus/essential_bus/inputs/1|
588|2|aircraft/0/systems/electrical_bus/essential_bus/voltage|
589|2|aircraft/0/systems/electrical_bus/essential_bus/amp_draw|
590|4|aircraft/0/systems/electrical_breaker/pfd_ess_bus_breaker/inputs/0|
591|1|aircraft/0/systems/electrical_breaker/pfd_ess_bus_breaker/state|
592|2|aircraft/0/systems/electrical_breaker/pfd_ess_bus_breaker/voltage|
593|2|aircraft/0/systems/electrical_breaker/pfd_ess_bus_breaker/amp_draw|
594|4|aircraft/0/systems/electrical_breaker/pfd_avionics_bus_1_breaker/inputs/0|
595|1|aircraft/0/systems/electrical_breaker/pfd_avionics_bus_1_breaker/state|
596|2|aircraft/0/systems/electrical_breaker/pfd_avionics_bus_1_breaker/voltage|
597|2|aircraft/0/systems/electrical_breaker/pfd_avionics_bus_1_breaker/amp_draw|
598|4|aircraft/0/systems/electrical_breaker/mfd_breaker/inputs/0|
599|1|aircraft/0/systems/electrical_breaker/mfd_breaker/state|
600|2|aircraft/0/systems/electrical_breaker/mfd_breaker/voltage|
601|2|aircraft/0/systems/electrical_breaker/mfd_breaker/amp_draw|
602|4|aircraft/0/systems/electrical_breaker/AHRS_breaker/inputs/0|
603|1|aircraft/0/systems/electrical_breaker/AHRS_breaker/state|
604|2|aircraft/0/systems/electrical_breaker/AHRS_breaker/voltage|
605|2|aircraft/0/systems/electrical_breaker/AHRS_breaker/amp_draw|
606|4|aircraft/0/systems/electrical_breaker/nav1_eng_breaker/inputs/0|
607|1|aircraft/0/systems/electrical_breaker/nav1_eng_breaker/state|
608|2|aircraft/0/systems/electrical_breaker/nav1_eng_breaker/voltage|
609|2|aircraft/0/systems/electrical_breaker/nav1_eng_breaker/amp_draw|
610|4|aircraft/0/systems/electrical_breaker/comm_1_breaker/inputs/0|
611|1|aircraft/0/systems/electrical_breaker/comm_1_breaker/state|
612|2|aircraft/0/systems/electrical_breaker/comm_1_breaker/voltage|
613|2|aircraft/0/systems/electrical_breaker/comm_1_breaker/amp_draw|
614|4|aircraft/0/systems/electrical_breaker/standby_ind_lights_breaker/inputs/0|
615|1|aircraft/0/systems/electrical_breaker/standby_ind_lights_breaker/state|
616|2|aircraft/0/systems/electrical_breaker/standby_ind_lights_breaker/voltage|
617|2|aircraft/0/systems/electrical_breaker/standby_ind_lights_breaker/amp_draw|
618|4|aircraft/0/systems/electrical_breaker/beacon_lights_breaker/inputs/0|
619|1|aircraft/0/systems/electrical_breaker/beacon_lights_breaker/state|
620|2|aircraft/0/systems/electrical_breaker/beacon_lights_breaker/voltage|
621|2|aircraft/0/systems/electrical_breaker/beacon_lights_breaker/amp_draw|
622|4|aircraft/0/systems/electrical_switch/beacon_lights_switch/inputs/0|
623|1|aircraft/0/systems/electrical_switch/beacon_lights_switch/state|
624|2|aircraft/0/systems/electrical_switch/beacon_lights_switch/voltage|
625|2|aircraft/0/systems/electrical_switch/beacon_lights_switch/amp_draw|
626|4|aircraft/0/systems/light_controller/beacon_lights_controller/inputs/0|
627|0|aircraft/0/systems/light_controller/beacon_lights_controller/state|
628|2|aircraft/0/systems/light_controller/beacon_lights_controller/voltage|
629|2|aircraft/0/systems/light_controller/beacon_lights_controller/amp_draw|
630|4|aircraft/0/systems/electrical_breaker/strobe_lights_breaker/inputs/0|
631|1|aircraft/0/systems/electrical_breaker/strobe_lights_breaker/state|
632|2|aircraft/0/systems/electrical_breaker/strobe_lights_breaker/voltage|
633|2|aircraft/0/systems/electrical_breaker/strobe_lights_breaker/amp_draw|
634|4|aircraft/0/systems/electrical_switch/strobe_lights_switch/inputs/0|
635|1|aircraft/0/systems/electrical_switch/strobe_lights_switch/state|
636|2|aircraft/0/systems/electrical_switch/strobe_lights_switch/voltage|
637|2|aircraft/0/systems/electrical_switch/strobe_lights_switch/amp_draw|
638|4|aircraft/0/systems/light_controller/strobe_lights_controller/inputs/0|
639|0|aircraft/0/systems/light_controller/strobe_lights_controller/state|
640|2|aircraft/0/systems/light_controller/strobe_lights_controller/voltage|
641|2|aircraft/0/systems/light_controller/strobe_lights_controller/amp_draw|
642|4|aircraft/0/systems/electrical_breaker/nav_lights_breaker/inputs/0|
643|1|aircraft/0/systems/electrical_breaker/nav_lights_breaker/state|
644|2|aircraft/0/systems/electrical_breaker/nav_lights_breaker/voltage|
645|2|aircraft/0/systems/electrical_breaker/nav_lights_breaker/amp_draw|
646|4|aircraft/0/systems/electrical_switch/nav_lights_switch/inputs/0|
647|1|aircraft/0/systems/electrical_switch/nav_lights_switch/state|
648|2|aircraft/0/systems/electrical_switch/nav_lights_switch/voltage|
649|2|aircraft/0/systems/electrical_switch/nav_lights_switch/amp_draw|
650|4|aircraft/0/systems/light_controller/nav_lights_controller/inputs/0|
651|0|aircraft/0/systems/light_controller/nav_lights_controller/state|
652|2|aircraft/0/systems/light_controller/nav_lights_controller/voltage|
653|2|aircraft/0/systems/light_controller/nav_lights_controller/amp_draw|
654|4|aircraft/0/systems/electrical_breaker/landing_lights_breaker/inputs/0|
655|1|aircraft/0/systems/electrical_breaker/landing_lights_breaker/state|
656|2|aircraft/0/systems/electrical_breaker/landing_lights_breaker/voltage|
657|2|aircraft/0/systems/electrical_breaker/landing_lights_breaker/amp_draw|
658|4|aircraft/0/systems/electrical_switch/landing_lights_switch/inputs/0|
659|1|aircraft/0/systems/electrical_switch/landing_lights_switch/state|
660|2|aircraft/0/systems/electrical_switch/landing_lights_switch/voltage|
661|2|aircraft/0/systems/electrical_switch/landing_lights_switch/amp_draw|
662|4|aircraft/0/systems/light_controller/landing_lights_controller/inputs/0|
663|0|aircraft/0/systems/light_controller/landing_lights_controller/state|
664|2|aircraft/0/systems/light_controller/landing_lights_controller/voltage|
665|2|aircraft/0/systems/light_controller/landing_lights_controller/amp_draw|
666|4|aircraft/0/systems/davtron_m803/1/inputs/0|
667|0|aircraft/0/systems/davtron_m803/1/on|
668|0|aircraft/0/systems/davtron_m803/1/isbooted|
669|1|aircraft/0/systems/davtron_m803/1/top_display_mode|
670|1|aircraft/0/systems/davtron_m803/1/bottom_display_mode|
671|1|aircraft/0/systems/davtron_m803/1/timer|
672|3|aircraft/0/systems/davtron_m803/1/timer_elapsed_time|
673|4|aircraft/0/systems/bendix_kx155a/0/inputs/0|
674|0|aircraft/0/systems/bendix_kx155a/0/on|
675|0|aircraft/0/systems/bendix_kx155a/0/isbooted|
676|1|aircraft/0/systems/bendix_kx155a/0/nav_index|
677|4|aircraft/0/systems/bendix_kx155a/1/inputs/0|
678|0|aircraft/0/systems/bendix_kx155a/1/on|
679|0|aircraft/0/systems/bendix_kx155a/1/isbooted|
680|1|aircraft/0/systems/bendix_kx155a/1/nav_index|
681|4|aircraft/0/systems/bendix_tk76c/0/inputs/0|
682|0|aircraft/0/systems/bendix_tk76c/0/on|
683|0|aircraft/0/systems/bendix_tk76c/0/isbooted|
684|4|aircraft/0/systems/bendix_kap140/0/inputs/0|
685|0|aircraft/0/systems/bendix_kap140/0/on|
686|0|aircraft/0/systems/bendix_kap140/0/isbooted|
687|1|aircraft/0/systems/bendix_kap140/0/page|
688|4|aircraft/0/systems/cessna_172_annunciator_panel/0/inputs/0|
689|0|aircraft/0/systems/cessna_172_annunciator_panel/0/on|
690|0|aircraft/0/systems/cessna_172_annunciator_panel/0/isbooted|
691|1|aircraft/0/systems/cessna_172_annunciator_panel/0/mode|
692|4|aircraft/0/systems/cessna_172_autopilot_source_panel/0/inputs/0|
693|0|aircraft/0/systems/cessna_172_autopilot_source_panel/0/on|
694|0|aircraft/0/systems/cessna_172_autopilot_source_panel/0/isbooted|
695|1|aircraft/0/systems/cessna_172_autopilot_source_panel/0/mode|
696|4|aircraft/0/systems/cessna_172_autopilot_source_panel/1/inputs/0|
697|0|aircraft/0/systems/cessna_172_autopilot_source_panel/1/on|
698|0|aircraft/0/systems/cessna_172_autopilot_source_panel/1/isbooted|
699|1|aircraft/0/systems/cessna_172_autopilot_source_panel/1/mode|
700|4|aircraft/0/systems/garmin_gnx375/2/inputs/0|
701|0|aircraft/0/systems/garmin_gnx375/2/on|
702|0|aircraft/0/systems/garmin_gnx375/2/isbooted|
703|1|aircraft/0/systems/garmin_gnx375/2/map_range|
704|4|aircraft/0/systems/garmin_gnx375/2/mfd_path|
705|1|aircraft/0/systems/garmin_gnx375/2/display_mode|
706|3|aircraft/0/systems/garmin_gnx375/2/boot_time|
707|1|aircraft/0/systems/garmin_gnx375/2/page|
708|4|aircraft/0/name|
709|4|aircraft/0/livery|
710|1|aircraft/0/configuration/flaps/stops|
711|4|aircraft/0/configuration/flaps/0/name|
712|4|aircraft/0/configuration/flaps/0/shortname|
713|2|aircraft/0/configuration/flaps/0/angle|
714|4|aircraft/0/configuration/flaps/1/name|
715|4|aircraft/0/configuration/flaps/1/shortname|
716|2|aircraft/0/configuration/flaps/1/angle|
717|4|aircraft/0/configuration/flaps/2/name|
718|4|aircraft/0/configuration/flaps/2/shortname|
719|2|aircraft/0/configuration/flaps/2/angle|
720|4|aircraft/0/configuration/flaps/3/name|
721|4|aircraft/0/configuration/flaps/3/shortname|
722|2|aircraft/0/configuration/flaps/3/angle|
723|0|aircraft/0/configuration/has_autopilot|
724|4|aircraft/0/configuration/spoiler_type|
725|2|aircraft/0/magnetic_variation|
726|2|aircraft/0/bank|
727|2|aircraft/0/pitch|
728|2|aircraft/0/indicated_airspeed|
729|2|aircraft/0/groundspeed|
730|2|aircraft/0/true_airspeed|
731|2|aircraft/0/mach_speed|
732|2|aircraft/0/altitude_msl|
733|2|aircraft/0/pressure_altitude|
734|2|aircraft/0/altitude_agl|
735|2|aircraft/0/vertical_speed|
736|2|aircraft/0/airspeed_change_rate|
737|2|aircraft/0/turn_rate|
738|2|aircraft/0/heading_magnetic|
739|2|aircraft/0/heading_true|
740|2|aircraft/0/2_minutes_turn_ratio|
741|2|aircraft/0/course|
742|2|aircraft/0/oat|
743|3|aircraft/0/latitude|
744|3|aircraft/0/longitude|
745|0|aircraft/0/is_on_ground|
746|0|aircraft/0/is_on_runway|
747|2|aircraft/0/g_force_y|
748|2|aircraft/0/g_force_x|
749|2|aircraft/0/configuration/mmo|
750|2|aircraft/0/configuration/vmo|
751|2|aircraft/0/configuration/vs|
752|2|aircraft/0/configuration/vs0|
753|2|aircraft/0/configuration/vno|
754|2|aircraft/0/configuration/bestglide|
755|2|aircraft/0/configuration/vfe|
756|2|aircraft/0/configuration/vx|
757|2|aircraft/0/configuration/vy|
758|0|aircraft/0/systems/light_controller/beacon_lights_controller|
759|0|aircraft/0/systems/strobe_lights_switch|
760|0|aircraft/0/systems/landing_lights_switch|
761|0|aircraft/0/systems/beacon_lights_switch|
762|1|aircraft/0/systems/fuel/fuel_tanks_count|
763|2|aircraft/0/systems/fuel/fuel_remaining|
764|2|aircraft/0/systems/fuel/tank/0/fuel_remaining|
765|2|aircraft/0/systems/fuel/tank/0/fuel_capacity|
766|2|aircraft/0/systems/fuel/tank/0/fuel_level|
767|2|aircraft/0/systems/fuel/tank/1/fuel_remaining|
768|2|aircraft/0/systems/fuel/tank/1/fuel_capacity|
769|2|aircraft/0/systems/fuel/tank/1/fuel_level|
770|1|aircraft/0/systems/fuel/selectors/0/selected_tank|
771|1|aircraft/0/systems/engines/engine_count|
772|2|aircraft/0/systems/engines/0/throttle_lever|
773|1|aircraft/0/systems/engines/0/state|
774|2|aircraft/0/systems/comm_radios/com_1/frequency|
775|4|aircraft/0/systems/comm_radios/com_1/name|
776|1|aircraft/0/systems/transponder/1/code|
777|3|aircraft/0/systems/nav_sources/gps/location/latitude|
778|3|aircraft/0/systems/nav_sources/gps/location/longitude|
779|3|aircraft/0/systems/nav_sources/gps/location/altitude|
780|2|aircraft/0/systems/nav_sources/gps/xtrack_distance|
781|2|aircraft/0/systems/nav_sources/gps/xtrack_angle|
782|2|aircraft/0/systems/nav_sources/gps/bearing|
783|2|aircraft/0/systems/nav_sources/gps/desired_track|
784|3|aircraft/0/systems/nav_sources/gps/distance_to_next|
785|4|aircraft/0/systems/nav_sources/gps/next_waypoint_name|
786|2|aircraft/0/systems/nav_sources/gps/ete_to_next|
787|4|aircraft/0/flightplan|
788|0|aircraft/0/systems/parking_brake/state|
789|1|aircraft/0/systems/spoilers/state|
790|1|aircraft/0/systems/flaps/state|
791|2|aircraft/0/systems/flaps/left_flap_angle|
792|2|aircraft/0/systems/flaps/right_flap_angle|
793|2|aircraft/0/systems/axes/roll|
794|2|aircraft/0/systems/axes/pitch|
795|2|aircraft/0/systems/axes/yaw|
796|0|aircraft/0/systems/landing_gear/lever_state|
797|1|aircraft/0/systems/landing_gear/state|
798|0|aircraft/0/systems/autopilot/approach/on|
799|0|aircraft/0/systems/autopilot/alt/on|
800|2|aircraft/0/systems/autopilot/alt/target|
801|0|aircraft/0/systems/autopilot/vs/on|
802|2|aircraft/0/systems/autopilot/vs/target|
803|2|aircraft/0/systems/autopilot/hdg/target|
804|0|aircraft/0/systems/autopilot/hdg/on|
805|1|aircraft/0/systems/autopilot/spd/mode|
806|2|aircraft/0/systems/autopilot/spd/target|
807|0|aircraft/0/systems/autopilot/spd/on|
808|0|aircraft/0/systems/autopilot/nav/on|
809|0|aircraft/0/systems/autopilot/on|
810|4|aircraft/0/flightplan/route|
811|5|aircraft/0/flightplan/last_update_time|
812|4|aircraft/0/flightplan/coordinates|
813|4|aircraft/0/flightplan/next_waypoint_name|
814|1|aircraft/0/flightplan/next_waypoint_index|
815|0|aircraft/0/systems/signs/seatbelt|
816|0|aircraft/0/systems/signs/no_smoking|
817|4|infiniteflight/cameras/0/group|
818|4|infiniteflight/cameras/0/name|
819|0|infiniteflight/cameras/0/is_inside|
820|3|infiniteflight/cameras/0/location/latitude|
821|3|infiniteflight/cameras/0/location/longitude|
822|4|infiniteflight/cameras/1/group|
823|4|infiniteflight/cameras/1/name|
824|0|infiniteflight/cameras/1/is_inside|
825|3|infiniteflight/cameras/1/location/latitude|
826|3|infiniteflight/cameras/1/location/longitude|
827|3|infiniteflight/cameras/1/x_angle|
828|3|infiniteflight/cameras/1/y_angle|
829|0|infiniteflight/cameras/1/angle_override|
830|4|infiniteflight/cameras/2/group|
831|4|infiniteflight/cameras/2/name|
832|0|infiniteflight/cameras/2/is_inside|
833|3|infiniteflight/cameras/2/location/latitude|
834|3|infiniteflight/cameras/2/location/longitude|
835|3|infiniteflight/cameras/2/x_angle|
836|3|infiniteflight/cameras/2/y_angle|
837|0|infiniteflight/cameras/2/angle_override|
838|4|infiniteflight/cameras/3/group|
839|4|infiniteflight/cameras/3/name|
840|0|infiniteflight/cameras/3/is_inside|
841|3|infiniteflight/cameras/3/location/latitude|
842|3|infiniteflight/cameras/3/location/longitude|
843|3|infiniteflight/cameras/3/x_angle|
844|3|infiniteflight/cameras/3/y_angle|
845|0|infiniteflight/cameras/3/angle_override|
846|4|infiniteflight/cameras/4/group|
847|4|infiniteflight/cameras/4/name|
848|0|infiniteflight/cameras/4/is_inside|
849|3|infiniteflight/cameras/4/location/latitude|
850|3|infiniteflight/cameras/4/location/longitude|
851|3|infiniteflight/cameras/4/x_angle|
852|3|infiniteflight/cameras/4/y_angle|
853|0|infiniteflight/cameras/4/angle_override|
854|4|infiniteflight/cameras/5/group|
855|4|infiniteflight/cameras/5/name|
856|0|infiniteflight/cameras/5/is_inside|
857|3|infiniteflight/cameras/5/location/latitude|
858|3|infiniteflight/cameras/5/location/longitude|
859|4|infiniteflight/cameras/6/group|
860|4|infiniteflight/cameras/6/name|
861|0|infiniteflight/cameras/6/is_inside|
862|3|infiniteflight/cameras/6/location/latitude|
863|3|infiniteflight/cameras/6/location/longitude|
864|4|infiniteflight/cameras/7/group|
865|4|infiniteflight/cameras/7/name|
866|0|infiniteflight/cameras/7/is_inside|
867|3|infiniteflight/cameras/7/location/latitude|
868|3|infiniteflight/cameras/7/location/longitude|
869|3|infiniteflight/cameras/7/x_angle|
870|3|infiniteflight/cameras/7/y_angle|
871|0|infiniteflight/cameras/7/angle_override|
872|4|infiniteflight/cameras/8/group|
873|4|infiniteflight/cameras/8/name|
874|0|infiniteflight/cameras/8/is_inside|
875|3|infiniteflight/cameras/8/location/latitude|
876|3|infiniteflight/cameras/8/location/longitude|
877|3|infiniteflight/cameras/8/x_angle|
878|3|infiniteflight/cameras/8/y_angle|
879|0|infiniteflight/cameras/8/angle_override|
880|4|infiniteflight/cameras/9/group|
881|4|infiniteflight/cameras/9/name|
882|0|infiniteflight/cameras/9/is_inside|
883|3|infiniteflight/cameras/9/location/latitude|
884|3|infiniteflight/cameras/9/location/longitude|
885|3|infiniteflight/cameras/9/x_angle|
886|3|infiniteflight/cameras/9/y_angle|
887|0|infiniteflight/cameras/9/angle_override|
888|4|infiniteflight/cameras/10/group|
889|4|infiniteflight/cameras/10/name|
890|0|infiniteflight/cameras/10/is_inside|
891|3|infiniteflight/cameras/10/location/latitude|
892|3|infiniteflight/cameras/10/location/longitude|
893|3|infiniteflight/cameras/10/x_angle|
894|3|infiniteflight/cameras/10/y_angle|
895|0|infiniteflight/cameras/10/angle_override|
896|4|infiniteflight/cameras/11/group|
897|4|infiniteflight/cameras/11/name|
898|0|infiniteflight/cameras/11/is_inside|
899|3|infiniteflight/cameras/11/location/latitude|
900|3|infiniteflight/cameras/11/location/longitude|
901|3|infiniteflight/cameras/11/x_angle|
902|3|infiniteflight/cameras/11/y_angle|
903|0|infiniteflight/cameras/11/angle_override|
904|4|infiniteflight/cameras/12/group|
905|4|infiniteflight/cameras/12/name|
906|0|infiniteflight/cameras/12/is_inside|
907|3|infiniteflight/cameras/12/location/latitude|
908|3|infiniteflight/cameras/12/location/longitude|
909|3|infiniteflight/cameras/12/x_angle|
910|3|infiniteflight/cameras/12/y_angle|
911|0|infiniteflight/cameras/12/angle_override|
912|4|infiniteflight/cameras/13/group|
913|4|infiniteflight/cameras/13/name|
914|0|infiniteflight/cameras/13/is_inside|
915|3|infiniteflight/cameras/13/location/latitude|
916|3|infiniteflight/cameras/13/location/longitude|
917|3|infiniteflight/cameras/13/x_angle|
918|3|infiniteflight/cameras/13/y_angle|
919|0|infiniteflight/cameras/13/angle_override|
920|4|infiniteflight/cameras/14/group|
921|4|infiniteflight/cameras/14/name|
922|0|infiniteflight/cameras/14/is_inside|
923|3|infiniteflight/cameras/14/location/latitude|
924|3|infiniteflight/cameras/14/location/longitude|
925|3|infiniteflight/cameras/14/x_angle|
926|3|infiniteflight/cameras/14/y_angle|
927|0|infiniteflight/cameras/14/angle_override|
928|4|infiniteflight/cameras/15/group|
929|4|infiniteflight/cameras/15/name|
930|0|infiniteflight/cameras/15/is_inside|
931|3|infiniteflight/cameras/15/location/latitude|
932|3|infiniteflight/cameras/15/location/longitude|
933|3|infiniteflight/cameras/15/x_angle|
934|3|infiniteflight/cameras/15/y_angle|
935|0|infiniteflight/cameras/15/angle_override|
936|4|infiniteflight/cameras/16/group|
937|4|infiniteflight/cameras/16/name|
938|0|infiniteflight/cameras/16/is_inside|
939|3|infiniteflight/cameras/16/location/latitude|
940|3|infiniteflight/cameras/16/location/longitude|
941|4|infiniteflight/cameras/17/group|
942|4|infiniteflight/cameras/17/name|
943|0|infiniteflight/cameras/17/is_inside|
944|3|infiniteflight/cameras/17/location/latitude|
945|3|infiniteflight/cameras/17/location/longitude|
946|4|infiniteflight/cameras/18/group|
947|4|infiniteflight/cameras/18/name|
948|0|infiniteflight/cameras/18/is_inside|
949|3|infiniteflight/cameras/18/location/latitude|
950|3|infiniteflight/cameras/18/location/longitude|
951|4|infiniteflight/cameras/19/group|
952|4|infiniteflight/cameras/19/name|
953|0|infiniteflight/cameras/19/is_inside|
954|3|infiniteflight/cameras/19/location/latitude|
955|3|infiniteflight/cameras/19/location/longitude|
956|3|infiniteflight/cameras/19/pitch|
957|3|infiniteflight/cameras/19/yaw|
958|3|infiniteflight/cameras/19/roll|
959|4|infiniteflight/cameras/20/group|
960|4|infiniteflight/cameras/20/name|
961|0|infiniteflight/cameras/20/is_inside|
962|3|infiniteflight/cameras/20/location/latitude|
963|3|infiniteflight/cameras/20/location/longitude|
964|4|infiniteflight/cameras/21/group|
965|4|infiniteflight/cameras/21/name|
966|0|infiniteflight/cameras/21/is_inside|
967|3|infiniteflight/cameras/21/location/latitude|
968|3|infiniteflight/cameras/21/location/longitude|
969|4|infiniteflight/cameras/22/group|
970|4|infiniteflight/cameras/22/name|
971|0|infiniteflight/cameras/22/is_inside|
972|3|infiniteflight/cameras/22/location/latitude|
973|3|infiniteflight/cameras/22/location/longitude|
974|3|infiniteflight/cameras/22/pitch|
975|3|infiniteflight/cameras/22/yaw|
976|3|infiniteflight/cameras/22/roll|
977|4|infiniteflight/current_camera|
1048576|-1|commands/ElevatorTrimUp|
1048577|-1|commands/ElevatorTrimDown|
1048578|-1|commands/ThrottleUpCommand|
1048579|-1|commands/ThrottleDownCommand|
1048580|-1|commands/SetThrottleCommand|
1048581|-1|commands/SetCockpitCamera|
1048582|-1|commands/SetVirtualCockpitCameraCommand|
1048583|-1|commands/SetFollowCameraCommand|
1048584|-1|commands/SetFlyByCamera|
1048585|-1|commands/SetOnboardCameraCommand|
1048586|-1|commands/SetTowerCameraCommand|
1048587|-1|commands/NextCamera|
1048588|-1|commands/PrevCamera|
1048589|-1|commands/CameraMoveLeft|
1048590|-1|commands/CameraMoveRight|
1048591|-1|commands/CameraMoveDown|
1048592|-1|commands/CameraMoveUp|
1048593|-1|commands/CameraMoveHorizontal|
1048594|-1|commands/CameraMoveVertical|
1048595|-1|commands/CameraZoomIn|
1048596|-1|commands/CameraZoomOut|
1048597|-1|commands/ShowATCWindowCommand|
1048598|-1|commands/ATCEntry1|
1048599|-1|commands/ATCEntry2|
1048600|-1|commands/ATCEntry3|
1048601|-1|commands/ATCEntry4|
1048602|-1|commands/ATCEntry5|
1048603|-1|commands/ATCEntry6|
1048604|-1|commands/ATCEntry7|
1048605|-1|commands/ATCEntry8|
1048606|-1|commands/ATCEntry9|
1048607|-1|commands/ATCEntry10|
1048608|-1|commands/Live.SetCOMFrequencies|
1048609|-1|commands/FlightPlan.AddWaypoints|
1048610|-1|commands/FlightPlan.Clear|
1048611|-1|commands/FlightPlan.ActivateLeg|
1048612|-1|commands/Brakes|
1048613|-1|commands/ParkingBrakes|
1048614|-1|commands/FlapsDown|
1048615|-1|commands/FlapsUp|
1048616|-1|commands/FlapsFullDown|
1048617|-1|commands/FlapsFullUp|
1048618|-1|commands/Aircraft.SetFlapState|
1048619|-1|commands/Spoilers|
1048620|-1|commands/LandingGear|
1048621|-1|commands/Pushback|
1048622|-1|commands/FuelDump|
1048623|-1|commands/ReverseThrust|
1048624|-1|commands/LandingLights|
1048625|-1|commands/TaxiLights|
1048626|-1|commands/StrobeLights|
1048627|-1|commands/BeaconLights|
1048628|-1|commands/NavLights|
1048629|-1|commands/SetLandingLightsState|
1048630|-1|commands/SetTaxiLightsState|
1048631|-1|commands/SetStrobeLightsState|
1048632|-1|commands/SetBeaconLightsState|
1048633|-1|commands/SetNavLightsState|
1048634|-1|commands/Autopilot.Toggle|
1048635|-1|commands/Autopilot.SetState|
1048636|-1|commands/Autopilot.SetHeading|
1048637|-1|commands/Autopilot.SetAltitude|
1048638|-1|commands/Autopilot.SetVS|
1048639|-1|commands/Autopilot.SetSpeed|
1048640|-1|commands/Autopilot.SetHeadingState|
1048641|-1|commands/Autopilot.SetAltitudeState|
1048642|-1|commands/Autopilot.SetVSState|
1048643|-1|commands/Autopilot.SetSpeedState|
1048644|-1|commands/Autopilot.SetApproachModeState|
1048645|-1|commands/Autopilot.SetLNavApproachModeState|
1048646|-1|commands/ToggleHUD|
1048647|-1|commands/ToggleHUD|
1048648|-1|commands/AutoStart|
1048649|-1|commands/TogglePause|
1048650|-1|commands/Engine.Start|
1048651|-1|commands/Engine.Stop|

### Other Aircraft

These docs may be updated with information about other aircraft. More information can be found on the [Infinite Flight Community forum API category](https://community.infiniteflight.com/c/thirdparty/api/40).

---
## API v1
## Connection

 1. To enable Infinite Flight command server, check `Enable Infinite Flight Connect` in `Settings > General`
 2. Infinite Flight will broadcast UDP packets on port `15000` containing its own IP address and Port.
Example message :
`{ "Address" : "192.168.0.11", "Port" : 10111 }`

 3. You must then establish a TCP connection on this given host and port

## Get Airplane State

This special command will request the airplane state from Infinite Flight. Response will be received on the same socket :
`{ "Command": "Airplane.GetState", "Parameters": []}`

## Command Messages

A command message is a object of the follwing form :


There are two types of command messages :
 - Commands : `{ "Command": "Commands.{CommandName}", "Parameters": []}`
 - Axis : `{ "Command": "NetworkJoystick.{AxisCommandName}", "Parameters": []}`


### List of commands

#### Joystick

Joystick axis use the `NetworkJoystick.SetAxisValue` command with two params :

 - Axis Name : `0` for Roll, `1` for Pitch
 - Value : a value between `-1024` and `1024`

Example :
```
{
  "Command": "NetworkJoystick.SetAxisValue",
  "Parameters": [ { "Name": 0, "Value": -340 } ]
}
```


#### Plane Systems

Commands to control various systems of the plane. Example, lower flaps down :
```
{
  "Command": "Commands.FlapsDown",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `Brakes` | Toggle brakes on/off |
| `ParkingBrakes` | Toggle parking brakes |
| `FlapsDown` | Decrement flaps |
| `FlapsUp` | Increment flaps |
| `FlapsFullDown` | Set flaps full |
| `FlapsFullUp` | Set flaps clean |
| `Aircraft.SetFlapState` | TODO : provide example |
| `Spoilers` | Switch between spoilers states (Off, Flight, Armed) |
| `LandingGear` | Toggle landing gear  |
| `Pushback` | Toggle Pushback (on/off) |
| `ReverseThrust` | Enable reverse (?) |
| `RollLeft` | |
| `RollRight` | |
| `PitchUp` | |
| `PitchDown` | |
| `ResetCommands` | |
| `ElevatorTrimUp` | Trim elevator up |
| `ElevatorTrimDown` | Trim elevator down |
| `ThrottleUpCommand` | |
| `ThrottleDownCommand` | |

#### Lights

Following commands toggle the state of a light. Example :
```
{
  "Command": "Commands.LandingLights",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `LandingLights` | Toggle landing lights |
| `TaxiLights` | Toggle taxi lights? |
| `StrobeLights` | Toggle strobe lights |
| `BeaconLights` | Toggle beacon lights |
| `NavLights` | Toggle nav lights  |

#### Camera
##### Camera Commands

Following commands can be used to control cameras. Example :
```
{
  "Command": "Commands.NextCamera",
  "Parameters": []
}
```

*For camera moves, see Axis commands below*

| Command  | Description  |
|---|---|
| `ToggleHUD` | Switch between HUD states (Full, Without map, disabled) |
| `NextCamera`| Switch to next camera |
| `PrevCamera` | Switch to previous camera |
| `CameraMoveLeft` | |
| `CameraMoveRight` | |
| `CameraMoveDown` | |
| `CameraMoveUp` | |
| `CameraMoveHorizontal` | |
| `CameraMoveVertical` | |
| `CameraZoomIn` | |
| `CameraZoomOut` | |
| `SetCockpitCamera` | |
| `SetVirtualCockpitCameraCommand` | |
| `SetFollowCameraCommand` | |
| `SetFlyByCamera` | |
| `SetOnboardCameraCommand` | |
| `SetTowerCameraCommand` | |

##### Camera Axis

Camera POV movements can be controlled via the following command :
```
{
  "Command": "NetworkJoystick.SetPOVState",
  "Parameters": [
    { "Name": "X", "Value": 0 },
    { "Name": "Y", "Value": 0 }
  ]
}
```

X and Y values can be either `-1`, `0` or `1`: they determine if the camera will move on each axis, either negatively or positively (or stay still on the given axis with the `0` value). For example, to move the POV to the left only horizontaly, use the following command :

```
{
  "Command": "NetworkJoystick.SetPOVState",
  "Parameters": [
    { "Name": "X", "Value": -1 },
    { "Name": "Y", "Value": 0 }
  ]
}
```

#### ATC

**Live only**
Allows you to send messages to ATC according to the options available on the ATC window. Example, call the ATC command #3 (as shown on the ATC window) :
```
{
  "Command": "Commands.ATCEntry3",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `ShowATCWindowCommand` | Show / Hide the ATC window |
| `ATCEntry1` | Choose the option #1 (or back)  |
| `ATCEntry2` | ... |
| `ATCEntry3` |  ...|
| `ATCEntry4` |  ...|
| `ATCEntry5` |  ...|
| `ATCEntry6` |  ...|
| `ATCEntry7` |  ...|
| `ATCEntry8` |  ...|
| `ATCEntry9` |  ...|
| `ATCEntry10` |  ...|
| `Live.SetCOMFrequencies` | TODO : describe this |

#### Autopilot and Flight Plan

Commands to set the value of an Autopilot param, or to toggle its state.
Examples :
Set HDG param to 270 :
```
{
  "Command": "Commands.Autopilot.SetHeading",
  "Parameters": [{ "Value": 270 }]
}
```

Enable HDG :
```
{
  "Command": "Commands.Autopilot.SetHeadingState",
  "Parameters": [{ "Value": true }]
}
```

Toggle Autopilot :
```
{
  "Command": "Commands.Autopilot.Toggle",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `Autopilot.Toggle` | Toggle autopilot |
| `Autopilot.SetState` | |
| `Autopilot.SetHeading` | Heading |
| `Autopilot.SetAltitude` | Altitude |
| `Autopilot.SetVS` | Vertical Speed |
| `Autopilot.SetSpeed` | Speed |
| `Autopilot.SetHeadingState` | |
| `Autopilot.SetAltitudeState` | |
| `Autopilot.SetVSState` | |
| `Autopilot.SetSpeedState` | |
| `Autopilot.SetApproachModeState` | |
| `FlightPlan.AddWaypoints` | Add waypoints to flightplan |
| `FlightPlan.Clear` | Clear flightplan |
| `FlightPlan.ActivateLeg` | |

### Simulator Commands

Control on simulator. Example, toggle play/pause :
```
{
  "Command": "Commands.TogglePause",
  "Parameters": []
}
```

| Command  | Description  |
|---|---|
| `TogglePause` | |
| `TakeScreenshot` | |
| `ToggleUber` | |
| `ToggleTime` | |
| `ToggleShader` | |
| `ToggleNormal` | |
| `Start Recording` | |
| `Stop Recording` | |

