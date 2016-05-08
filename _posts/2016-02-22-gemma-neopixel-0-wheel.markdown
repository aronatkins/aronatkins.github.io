---
title: 'Gemma+NeoPixel 0: Wheel'
date: '2016-02-22'
layout: post
tags: arduino gemma code neopixel adafruit
style: |
    table { border-collapse: collapse; }
    th,td { text-align: right; }
    tbody tr td:first-child { border-left: 1px solid #000; }
    tbody tr th:first-child { border-left: 1px solid #000; }
    tbody tr td:last-child { border-right: 1px solid #000; }
    tbody tr th:last-child { border-right: 1px solid #000; }
    tbody tr:first-child td { border-top: 1px solid #000; }
    tbody tr:first-child th { border-top: 1px solid #000; }
    tbody tr:last-child td { border-bottom: 1px solid #000; }
    tbody tr:last-child th { border-bottom: 1px solid #000; }
---

This post describes the
[`Wheel`](https://github.com/adafruit/Adafruit_NeoPixel/blob/be0d7706e196dd8479e4573038785bad0b1e6726/examples/strandtest/strandtest.ino)
function that is part of the
[strandtest](https://github.com/adafruit/Adafruit_NeoPixel/tree/be0d7706e196dd8479e4573038785bad0b1e6726/examples/strandtest)
example included within the [NeoPixel library](https://github.com/adafruit/Adafruit_NeoPixel).

I'm writing it as a preamble to a planned series of articles about my
experiments with an Arduino [Gemma](https://www.adafruit.com/products/1222)
and a NeoPixel ring with
[16 RGB LEDs](https://www.adafruit.com/products/1463).

The `Wheel` function takes a value from 0 to 255 (inclusive) and maps that
onto a red-green-blue color wheel. Internally, it computes a value for the
red, green, and blue components of the result. Those three parts are combined
by the `strip.Color` function to return a 32-bit value representing that
color.

> The `Wheel` function may eventually be included in the NeoPixel library, but
> for now, include it in each of your sketches.

```cpp
uint32_t Wheel(byte WheelPos) {
  WheelPos = 255 - WheelPos;
  if(WheelPos < 85) {
    return strip.Color(255 - WheelPos * 3, 0, WheelPos * 3);
  }
  if(WheelPos < 170) {
    WheelPos -= 85;
    return strip.Color(0, WheelPos * 3, 255 - WheelPos * 3);
  }
  WheelPos -= 170;
  return strip.Color(WheelPos * 3, 255 - WheelPos * 3, 0);
}
```

From a quick code reading, we can see that the numbers from 0 to 255 are
divided into three sections: 0-85, 86-170, and 171-255. But what we care about
more than the math are the colors that result.

Let's build a table showing how an input value is transformed. I wrote a
little
[Python code](https://github.com/aronatkins/bits-bytes-and-lights/blob/2498dcf3472732ed5761af9fce068ee77b4fd8de/NeoPixel/color-wheel.py)
to take each number from 0 to 255 and give the resulting red, green, and blue
values. In addition, I added the HTML hex color code and the color itself.
Maybe it'll help you when incorporating `Wheel` into your own designs.

> The color wheel only includes red, green, and blue combinations. There is no
> black and no white. You may need to use other techniques or in addition
> to the `Wheel` function.

Get ready for some scrolling!

<table>
<tbody>
<tr><th>position</th><th>0</th><th>1</th><th>2</th><th>3</th><th>4</th><th>5</th><th>6</th><th>7</th></tr>
<tr><th>Red</th><td>255</td><td>252</td><td>249</td><td>246</td><td>243</td><td>240</td><td>237</td><td>234</td></tr>
<tr><th>Green</th><td>0</td><td>3</td><td>6</td><td>9</td><td>12</td><td>15</td><td>18</td><td>21</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#ff0000<tt></td><td><tt>#fc0300<tt></td><td><tt>#f90600<tt></td><td><tt>#f60900<tt></td><td><tt>#f30c00<tt></td><td><tt>#f00f00<tt></td><td><tt>#ed1200<tt></td><td><tt>#ea1500<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#ff0000;'></td><td style='background-color:#fc0300;'></td><td style='background-color:#f90600;'></td><td style='background-color:#f60900;'></td><td style='background-color:#f30c00;'></td><td style='background-color:#f00f00;'></td><td style='background-color:#ed1200;'></td><td style='background-color:#ea1500;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>8</th><th>9</th><th>10</th><th>11</th><th>12</th><th>13</th><th>14</th><th>15</th></tr>
<tr><th>Red</th><td>231</td><td>228</td><td>225</td><td>222</td><td>219</td><td>216</td><td>213</td><td>210</td></tr>
<tr><th>Green</th><td>24</td><td>27</td><td>30</td><td>33</td><td>36</td><td>39</td><td>42</td><td>45</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#e71800<tt></td><td><tt>#e41b00<tt></td><td><tt>#e11e00<tt></td><td><tt>#de2100<tt></td><td><tt>#db2400<tt></td><td><tt>#d82700<tt></td><td><tt>#d52a00<tt></td><td><tt>#d22d00<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#e71800;'></td><td style='background-color:#e41b00;'></td><td style='background-color:#e11e00;'></td><td style='background-color:#de2100;'></td><td style='background-color:#db2400;'></td><td style='background-color:#d82700;'></td><td style='background-color:#d52a00;'></td><td style='background-color:#d22d00;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>16</th><th>17</th><th>18</th><th>19</th><th>20</th><th>21</th><th>22</th><th>23</th></tr>
<tr><th>Red</th><td>207</td><td>204</td><td>201</td><td>198</td><td>195</td><td>192</td><td>189</td><td>186</td></tr>
<tr><th>Green</th><td>48</td><td>51</td><td>54</td><td>57</td><td>60</td><td>63</td><td>66</td><td>69</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#cf3000<tt></td><td><tt>#cc3300<tt></td><td><tt>#c93600<tt></td><td><tt>#c63900<tt></td><td><tt>#c33c00<tt></td><td><tt>#c03f00<tt></td><td><tt>#bd4200<tt></td><td><tt>#ba4500<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#cf3000;'></td><td style='background-color:#cc3300;'></td><td style='background-color:#c93600;'></td><td style='background-color:#c63900;'></td><td style='background-color:#c33c00;'></td><td style='background-color:#c03f00;'></td><td style='background-color:#bd4200;'></td><td style='background-color:#ba4500;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>24</th><th>25</th><th>26</th><th>27</th><th>28</th><th>29</th><th>30</th><th>31</th></tr>
<tr><th>Red</th><td>183</td><td>180</td><td>177</td><td>174</td><td>171</td><td>168</td><td>165</td><td>162</td></tr>
<tr><th>Green</th><td>72</td><td>75</td><td>78</td><td>81</td><td>84</td><td>87</td><td>90</td><td>93</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#b74800<tt></td><td><tt>#b44b00<tt></td><td><tt>#b14e00<tt></td><td><tt>#ae5100<tt></td><td><tt>#ab5400<tt></td><td><tt>#a85700<tt></td><td><tt>#a55a00<tt></td><td><tt>#a25d00<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#b74800;'></td><td style='background-color:#b44b00;'></td><td style='background-color:#b14e00;'></td><td style='background-color:#ae5100;'></td><td style='background-color:#ab5400;'></td><td style='background-color:#a85700;'></td><td style='background-color:#a55a00;'></td><td style='background-color:#a25d00;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>32</th><th>33</th><th>34</th><th>35</th><th>36</th><th>37</th><th>38</th><th>39</th></tr>
<tr><th>Red</th><td>159</td><td>156</td><td>153</td><td>150</td><td>147</td><td>144</td><td>141</td><td>138</td></tr>
<tr><th>Green</th><td>96</td><td>99</td><td>102</td><td>105</td><td>108</td><td>111</td><td>114</td><td>117</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#9f6000<tt></td><td><tt>#9c6300<tt></td><td><tt>#996600<tt></td><td><tt>#966900<tt></td><td><tt>#936c00<tt></td><td><tt>#906f00<tt></td><td><tt>#8d7200<tt></td><td><tt>#8a7500<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#9f6000;'></td><td style='background-color:#9c6300;'></td><td style='background-color:#996600;'></td><td style='background-color:#966900;'></td><td style='background-color:#936c00;'></td><td style='background-color:#906f00;'></td><td style='background-color:#8d7200;'></td><td style='background-color:#8a7500;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>40</th><th>41</th><th>42</th><th>43</th><th>44</th><th>45</th><th>46</th><th>47</th></tr>
<tr><th>Red</th><td>135</td><td>132</td><td>129</td><td>126</td><td>123</td><td>120</td><td>117</td><td>114</td></tr>
<tr><th>Green</th><td>120</td><td>123</td><td>126</td><td>129</td><td>132</td><td>135</td><td>138</td><td>141</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#877800<tt></td><td><tt>#847b00<tt></td><td><tt>#817e00<tt></td><td><tt>#7e8100<tt></td><td><tt>#7b8400<tt></td><td><tt>#788700<tt></td><td><tt>#758a00<tt></td><td><tt>#728d00<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#877800;'></td><td style='background-color:#847b00;'></td><td style='background-color:#817e00;'></td><td style='background-color:#7e8100;'></td><td style='background-color:#7b8400;'></td><td style='background-color:#788700;'></td><td style='background-color:#758a00;'></td><td style='background-color:#728d00;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>48</th><th>49</th><th>50</th><th>51</th><th>52</th><th>53</th><th>54</th><th>55</th></tr>
<tr><th>Red</th><td>111</td><td>108</td><td>105</td><td>102</td><td>99</td><td>96</td><td>93</td><td>90</td></tr>
<tr><th>Green</th><td>144</td><td>147</td><td>150</td><td>153</td><td>156</td><td>159</td><td>162</td><td>165</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#6f9000<tt></td><td><tt>#6c9300<tt></td><td><tt>#699600<tt></td><td><tt>#669900<tt></td><td><tt>#639c00<tt></td><td><tt>#609f00<tt></td><td><tt>#5da200<tt></td><td><tt>#5aa500<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#6f9000;'></td><td style='background-color:#6c9300;'></td><td style='background-color:#699600;'></td><td style='background-color:#669900;'></td><td style='background-color:#639c00;'></td><td style='background-color:#609f00;'></td><td style='background-color:#5da200;'></td><td style='background-color:#5aa500;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>56</th><th>57</th><th>58</th><th>59</th><th>60</th><th>61</th><th>62</th><th>63</th></tr>
<tr><th>Red</th><td>87</td><td>84</td><td>81</td><td>78</td><td>75</td><td>72</td><td>69</td><td>66</td></tr>
<tr><th>Green</th><td>168</td><td>171</td><td>174</td><td>177</td><td>180</td><td>183</td><td>186</td><td>189</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#57a800<tt></td><td><tt>#54ab00<tt></td><td><tt>#51ae00<tt></td><td><tt>#4eb100<tt></td><td><tt>#4bb400<tt></td><td><tt>#48b700<tt></td><td><tt>#45ba00<tt></td><td><tt>#42bd00<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#57a800;'></td><td style='background-color:#54ab00;'></td><td style='background-color:#51ae00;'></td><td style='background-color:#4eb100;'></td><td style='background-color:#4bb400;'></td><td style='background-color:#48b700;'></td><td style='background-color:#45ba00;'></td><td style='background-color:#42bd00;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>64</th><th>65</th><th>66</th><th>67</th><th>68</th><th>69</th><th>70</th><th>71</th></tr>
<tr><th>Red</th><td>63</td><td>60</td><td>57</td><td>54</td><td>51</td><td>48</td><td>45</td><td>42</td></tr>
<tr><th>Green</th><td>192</td><td>195</td><td>198</td><td>201</td><td>204</td><td>207</td><td>210</td><td>213</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#3fc000<tt></td><td><tt>#3cc300<tt></td><td><tt>#39c600<tt></td><td><tt>#36c900<tt></td><td><tt>#33cc00<tt></td><td><tt>#30cf00<tt></td><td><tt>#2dd200<tt></td><td><tt>#2ad500<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#3fc000;'></td><td style='background-color:#3cc300;'></td><td style='background-color:#39c600;'></td><td style='background-color:#36c900;'></td><td style='background-color:#33cc00;'></td><td style='background-color:#30cf00;'></td><td style='background-color:#2dd200;'></td><td style='background-color:#2ad500;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>72</th><th>73</th><th>74</th><th>75</th><th>76</th><th>77</th><th>78</th><th>79</th></tr>
<tr><th>Red</th><td>39</td><td>36</td><td>33</td><td>30</td><td>27</td><td>24</td><td>21</td><td>18</td></tr>
<tr><th>Green</th><td>216</td><td>219</td><td>222</td><td>225</td><td>228</td><td>231</td><td>234</td><td>237</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#27d800<tt></td><td><tt>#24db00<tt></td><td><tt>#21de00<tt></td><td><tt>#1ee100<tt></td><td><tt>#1be400<tt></td><td><tt>#18e700<tt></td><td><tt>#15ea00<tt></td><td><tt>#12ed00<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#27d800;'></td><td style='background-color:#24db00;'></td><td style='background-color:#21de00;'></td><td style='background-color:#1ee100;'></td><td style='background-color:#1be400;'></td><td style='background-color:#18e700;'></td><td style='background-color:#15ea00;'></td><td style='background-color:#12ed00;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>80</th><th>81</th><th>82</th><th>83</th><th>84</th><th>85</th><th>86</th><th>87</th></tr>
<tr><th>Red</th><td>15</td><td>12</td><td>9</td><td>6</td><td>3</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>240</td><td>243</td><td>246</td><td>249</td><td>252</td><td>255</td><td>252</td><td>249</td></tr>
<tr><th>Blue</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>3</td><td>6</td></tr>
<tr><th>HTML color code</th><td><tt>#0ff000<tt></td><td><tt>#0cf300<tt></td><td><tt>#09f600<tt></td><td><tt>#06f900<tt></td><td><tt>#03fc00<tt></td><td><tt>#00ff00<tt></td><td><tt>#00fc03<tt></td><td><tt>#00f906<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#0ff000;'></td><td style='background-color:#0cf300;'></td><td style='background-color:#09f600;'></td><td style='background-color:#06f900;'></td><td style='background-color:#03fc00;'></td><td style='background-color:#00ff00;'></td><td style='background-color:#00fc03;'></td><td style='background-color:#00f906;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>88</th><th>89</th><th>90</th><th>91</th><th>92</th><th>93</th><th>94</th><th>95</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>246</td><td>243</td><td>240</td><td>237</td><td>234</td><td>231</td><td>228</td><td>225</td></tr>
<tr><th>Blue</th><td>9</td><td>12</td><td>15</td><td>18</td><td>21</td><td>24</td><td>27</td><td>30</td></tr>
<tr><th>HTML color code</th><td><tt>#00f609<tt></td><td><tt>#00f30c<tt></td><td><tt>#00f00f<tt></td><td><tt>#00ed12<tt></td><td><tt>#00ea15<tt></td><td><tt>#00e718<tt></td><td><tt>#00e41b<tt></td><td><tt>#00e11e<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#00f609;'></td><td style='background-color:#00f30c;'></td><td style='background-color:#00f00f;'></td><td style='background-color:#00ed12;'></td><td style='background-color:#00ea15;'></td><td style='background-color:#00e718;'></td><td style='background-color:#00e41b;'></td><td style='background-color:#00e11e;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>96</th><th>97</th><th>98</th><th>99</th><th>100</th><th>101</th><th>102</th><th>103</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>222</td><td>219</td><td>216</td><td>213</td><td>210</td><td>207</td><td>204</td><td>201</td></tr>
<tr><th>Blue</th><td>33</td><td>36</td><td>39</td><td>42</td><td>45</td><td>48</td><td>51</td><td>54</td></tr>
<tr><th>HTML color code</th><td><tt>#00de21<tt></td><td><tt>#00db24<tt></td><td><tt>#00d827<tt></td><td><tt>#00d52a<tt></td><td><tt>#00d22d<tt></td><td><tt>#00cf30<tt></td><td><tt>#00cc33<tt></td><td><tt>#00c936<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#00de21;'></td><td style='background-color:#00db24;'></td><td style='background-color:#00d827;'></td><td style='background-color:#00d52a;'></td><td style='background-color:#00d22d;'></td><td style='background-color:#00cf30;'></td><td style='background-color:#00cc33;'></td><td style='background-color:#00c936;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>104</th><th>105</th><th>106</th><th>107</th><th>108</th><th>109</th><th>110</th><th>111</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>198</td><td>195</td><td>192</td><td>189</td><td>186</td><td>183</td><td>180</td><td>177</td></tr>
<tr><th>Blue</th><td>57</td><td>60</td><td>63</td><td>66</td><td>69</td><td>72</td><td>75</td><td>78</td></tr>
<tr><th>HTML color code</th><td><tt>#00c639<tt></td><td><tt>#00c33c<tt></td><td><tt>#00c03f<tt></td><td><tt>#00bd42<tt></td><td><tt>#00ba45<tt></td><td><tt>#00b748<tt></td><td><tt>#00b44b<tt></td><td><tt>#00b14e<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#00c639;'></td><td style='background-color:#00c33c;'></td><td style='background-color:#00c03f;'></td><td style='background-color:#00bd42;'></td><td style='background-color:#00ba45;'></td><td style='background-color:#00b748;'></td><td style='background-color:#00b44b;'></td><td style='background-color:#00b14e;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>112</th><th>113</th><th>114</th><th>115</th><th>116</th><th>117</th><th>118</th><th>119</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>174</td><td>171</td><td>168</td><td>165</td><td>162</td><td>159</td><td>156</td><td>153</td></tr>
<tr><th>Blue</th><td>81</td><td>84</td><td>87</td><td>90</td><td>93</td><td>96</td><td>99</td><td>102</td></tr>
<tr><th>HTML color code</th><td><tt>#00ae51<tt></td><td><tt>#00ab54<tt></td><td><tt>#00a857<tt></td><td><tt>#00a55a<tt></td><td><tt>#00a25d<tt></td><td><tt>#009f60<tt></td><td><tt>#009c63<tt></td><td><tt>#009966<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#00ae51;'></td><td style='background-color:#00ab54;'></td><td style='background-color:#00a857;'></td><td style='background-color:#00a55a;'></td><td style='background-color:#00a25d;'></td><td style='background-color:#009f60;'></td><td style='background-color:#009c63;'></td><td style='background-color:#009966;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>120</th><th>121</th><th>122</th><th>123</th><th>124</th><th>125</th><th>126</th><th>127</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>150</td><td>147</td><td>144</td><td>141</td><td>138</td><td>135</td><td>132</td><td>129</td></tr>
<tr><th>Blue</th><td>105</td><td>108</td><td>111</td><td>114</td><td>117</td><td>120</td><td>123</td><td>126</td></tr>
<tr><th>HTML color code</th><td><tt>#009669<tt></td><td><tt>#00936c<tt></td><td><tt>#00906f<tt></td><td><tt>#008d72<tt></td><td><tt>#008a75<tt></td><td><tt>#008778<tt></td><td><tt>#00847b<tt></td><td><tt>#00817e<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#009669;'></td><td style='background-color:#00936c;'></td><td style='background-color:#00906f;'></td><td style='background-color:#008d72;'></td><td style='background-color:#008a75;'></td><td style='background-color:#008778;'></td><td style='background-color:#00847b;'></td><td style='background-color:#00817e;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>128</th><th>129</th><th>130</th><th>131</th><th>132</th><th>133</th><th>134</th><th>135</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>126</td><td>123</td><td>120</td><td>117</td><td>114</td><td>111</td><td>108</td><td>105</td></tr>
<tr><th>Blue</th><td>129</td><td>132</td><td>135</td><td>138</td><td>141</td><td>144</td><td>147</td><td>150</td></tr>
<tr><th>HTML color code</th><td><tt>#007e81<tt></td><td><tt>#007b84<tt></td><td><tt>#007887<tt></td><td><tt>#00758a<tt></td><td><tt>#00728d<tt></td><td><tt>#006f90<tt></td><td><tt>#006c93<tt></td><td><tt>#006996<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#007e81;'></td><td style='background-color:#007b84;'></td><td style='background-color:#007887;'></td><td style='background-color:#00758a;'></td><td style='background-color:#00728d;'></td><td style='background-color:#006f90;'></td><td style='background-color:#006c93;'></td><td style='background-color:#006996;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>136</th><th>137</th><th>138</th><th>139</th><th>140</th><th>141</th><th>142</th><th>143</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>102</td><td>99</td><td>96</td><td>93</td><td>90</td><td>87</td><td>84</td><td>81</td></tr>
<tr><th>Blue</th><td>153</td><td>156</td><td>159</td><td>162</td><td>165</td><td>168</td><td>171</td><td>174</td></tr>
<tr><th>HTML color code</th><td><tt>#006699<tt></td><td><tt>#00639c<tt></td><td><tt>#00609f<tt></td><td><tt>#005da2<tt></td><td><tt>#005aa5<tt></td><td><tt>#0057a8<tt></td><td><tt>#0054ab<tt></td><td><tt>#0051ae<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#006699;'></td><td style='background-color:#00639c;'></td><td style='background-color:#00609f;'></td><td style='background-color:#005da2;'></td><td style='background-color:#005aa5;'></td><td style='background-color:#0057a8;'></td><td style='background-color:#0054ab;'></td><td style='background-color:#0051ae;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>144</th><th>145</th><th>146</th><th>147</th><th>148</th><th>149</th><th>150</th><th>151</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>78</td><td>75</td><td>72</td><td>69</td><td>66</td><td>63</td><td>60</td><td>57</td></tr>
<tr><th>Blue</th><td>177</td><td>180</td><td>183</td><td>186</td><td>189</td><td>192</td><td>195</td><td>198</td></tr>
<tr><th>HTML color code</th><td><tt>#004eb1<tt></td><td><tt>#004bb4<tt></td><td><tt>#0048b7<tt></td><td><tt>#0045ba<tt></td><td><tt>#0042bd<tt></td><td><tt>#003fc0<tt></td><td><tt>#003cc3<tt></td><td><tt>#0039c6<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#004eb1;'></td><td style='background-color:#004bb4;'></td><td style='background-color:#0048b7;'></td><td style='background-color:#0045ba;'></td><td style='background-color:#0042bd;'></td><td style='background-color:#003fc0;'></td><td style='background-color:#003cc3;'></td><td style='background-color:#0039c6;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>152</th><th>153</th><th>154</th><th>155</th><th>156</th><th>157</th><th>158</th><th>159</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>54</td><td>51</td><td>48</td><td>45</td><td>42</td><td>39</td><td>36</td><td>33</td></tr>
<tr><th>Blue</th><td>201</td><td>204</td><td>207</td><td>210</td><td>213</td><td>216</td><td>219</td><td>222</td></tr>
<tr><th>HTML color code</th><td><tt>#0036c9<tt></td><td><tt>#0033cc<tt></td><td><tt>#0030cf<tt></td><td><tt>#002dd2<tt></td><td><tt>#002ad5<tt></td><td><tt>#0027d8<tt></td><td><tt>#0024db<tt></td><td><tt>#0021de<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#0036c9;'></td><td style='background-color:#0033cc;'></td><td style='background-color:#0030cf;'></td><td style='background-color:#002dd2;'></td><td style='background-color:#002ad5;'></td><td style='background-color:#0027d8;'></td><td style='background-color:#0024db;'></td><td style='background-color:#0021de;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>160</th><th>161</th><th>162</th><th>163</th><th>164</th><th>165</th><th>166</th><th>167</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Green</th><td>30</td><td>27</td><td>24</td><td>21</td><td>18</td><td>15</td><td>12</td><td>9</td></tr>
<tr><th>Blue</th><td>225</td><td>228</td><td>231</td><td>234</td><td>237</td><td>240</td><td>243</td><td>246</td></tr>
<tr><th>HTML color code</th><td><tt>#001ee1<tt></td><td><tt>#001be4<tt></td><td><tt>#0018e7<tt></td><td><tt>#0015ea<tt></td><td><tt>#0012ed<tt></td><td><tt>#000ff0<tt></td><td><tt>#000cf3<tt></td><td><tt>#0009f6<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#001ee1;'></td><td style='background-color:#001be4;'></td><td style='background-color:#0018e7;'></td><td style='background-color:#0015ea;'></td><td style='background-color:#0012ed;'></td><td style='background-color:#000ff0;'></td><td style='background-color:#000cf3;'></td><td style='background-color:#0009f6;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>168</th><th>169</th><th>170</th><th>171</th><th>172</th><th>173</th><th>174</th><th>175</th></tr>
<tr><th>Red</th><td>0</td><td>0</td><td>0</td><td>3</td><td>6</td><td>9</td><td>12</td><td>15</td></tr>
<tr><th>Green</th><td>6</td><td>3</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>249</td><td>252</td><td>255</td><td>252</td><td>249</td><td>246</td><td>243</td><td>240</td></tr>
<tr><th>HTML color code</th><td><tt>#0006f9<tt></td><td><tt>#0003fc<tt></td><td><tt>#0000ff<tt></td><td><tt>#0300fc<tt></td><td><tt>#0600f9<tt></td><td><tt>#0900f6<tt></td><td><tt>#0c00f3<tt></td><td><tt>#0f00f0<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#0006f9;'></td><td style='background-color:#0003fc;'></td><td style='background-color:#0000ff;'></td><td style='background-color:#0300fc;'></td><td style='background-color:#0600f9;'></td><td style='background-color:#0900f6;'></td><td style='background-color:#0c00f3;'></td><td style='background-color:#0f00f0;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>176</th><th>177</th><th>178</th><th>179</th><th>180</th><th>181</th><th>182</th><th>183</th></tr>
<tr><th>Red</th><td>18</td><td>21</td><td>24</td><td>27</td><td>30</td><td>33</td><td>36</td><td>39</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>237</td><td>234</td><td>231</td><td>228</td><td>225</td><td>222</td><td>219</td><td>216</td></tr>
<tr><th>HTML color code</th><td><tt>#1200ed<tt></td><td><tt>#1500ea<tt></td><td><tt>#1800e7<tt></td><td><tt>#1b00e4<tt></td><td><tt>#1e00e1<tt></td><td><tt>#2100de<tt></td><td><tt>#2400db<tt></td><td><tt>#2700d8<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#1200ed;'></td><td style='background-color:#1500ea;'></td><td style='background-color:#1800e7;'></td><td style='background-color:#1b00e4;'></td><td style='background-color:#1e00e1;'></td><td style='background-color:#2100de;'></td><td style='background-color:#2400db;'></td><td style='background-color:#2700d8;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>184</th><th>185</th><th>186</th><th>187</th><th>188</th><th>189</th><th>190</th><th>191</th></tr>
<tr><th>Red</th><td>42</td><td>45</td><td>48</td><td>51</td><td>54</td><td>57</td><td>60</td><td>63</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>213</td><td>210</td><td>207</td><td>204</td><td>201</td><td>198</td><td>195</td><td>192</td></tr>
<tr><th>HTML color code</th><td><tt>#2a00d5<tt></td><td><tt>#2d00d2<tt></td><td><tt>#3000cf<tt></td><td><tt>#3300cc<tt></td><td><tt>#3600c9<tt></td><td><tt>#3900c6<tt></td><td><tt>#3c00c3<tt></td><td><tt>#3f00c0<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#2a00d5;'></td><td style='background-color:#2d00d2;'></td><td style='background-color:#3000cf;'></td><td style='background-color:#3300cc;'></td><td style='background-color:#3600c9;'></td><td style='background-color:#3900c6;'></td><td style='background-color:#3c00c3;'></td><td style='background-color:#3f00c0;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>192</th><th>193</th><th>194</th><th>195</th><th>196</th><th>197</th><th>198</th><th>199</th></tr>
<tr><th>Red</th><td>66</td><td>69</td><td>72</td><td>75</td><td>78</td><td>81</td><td>84</td><td>87</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>189</td><td>186</td><td>183</td><td>180</td><td>177</td><td>174</td><td>171</td><td>168</td></tr>
<tr><th>HTML color code</th><td><tt>#4200bd<tt></td><td><tt>#4500ba<tt></td><td><tt>#4800b7<tt></td><td><tt>#4b00b4<tt></td><td><tt>#4e00b1<tt></td><td><tt>#5100ae<tt></td><td><tt>#5400ab<tt></td><td><tt>#5700a8<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#4200bd;'></td><td style='background-color:#4500ba;'></td><td style='background-color:#4800b7;'></td><td style='background-color:#4b00b4;'></td><td style='background-color:#4e00b1;'></td><td style='background-color:#5100ae;'></td><td style='background-color:#5400ab;'></td><td style='background-color:#5700a8;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>200</th><th>201</th><th>202</th><th>203</th><th>204</th><th>205</th><th>206</th><th>207</th></tr>
<tr><th>Red</th><td>90</td><td>93</td><td>96</td><td>99</td><td>102</td><td>105</td><td>108</td><td>111</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>165</td><td>162</td><td>159</td><td>156</td><td>153</td><td>150</td><td>147</td><td>144</td></tr>
<tr><th>HTML color code</th><td><tt>#5a00a5<tt></td><td><tt>#5d00a2<tt></td><td><tt>#60009f<tt></td><td><tt>#63009c<tt></td><td><tt>#660099<tt></td><td><tt>#690096<tt></td><td><tt>#6c0093<tt></td><td><tt>#6f0090<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#5a00a5;'></td><td style='background-color:#5d00a2;'></td><td style='background-color:#60009f;'></td><td style='background-color:#63009c;'></td><td style='background-color:#660099;'></td><td style='background-color:#690096;'></td><td style='background-color:#6c0093;'></td><td style='background-color:#6f0090;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>208</th><th>209</th><th>210</th><th>211</th><th>212</th><th>213</th><th>214</th><th>215</th></tr>
<tr><th>Red</th><td>114</td><td>117</td><td>120</td><td>123</td><td>126</td><td>129</td><td>132</td><td>135</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>141</td><td>138</td><td>135</td><td>132</td><td>129</td><td>126</td><td>123</td><td>120</td></tr>
<tr><th>HTML color code</th><td><tt>#72008d<tt></td><td><tt>#75008a<tt></td><td><tt>#780087<tt></td><td><tt>#7b0084<tt></td><td><tt>#7e0081<tt></td><td><tt>#81007e<tt></td><td><tt>#84007b<tt></td><td><tt>#870078<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#72008d;'></td><td style='background-color:#75008a;'></td><td style='background-color:#780087;'></td><td style='background-color:#7b0084;'></td><td style='background-color:#7e0081;'></td><td style='background-color:#81007e;'></td><td style='background-color:#84007b;'></td><td style='background-color:#870078;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>216</th><th>217</th><th>218</th><th>219</th><th>220</th><th>221</th><th>222</th><th>223</th></tr>
<tr><th>Red</th><td>138</td><td>141</td><td>144</td><td>147</td><td>150</td><td>153</td><td>156</td><td>159</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>117</td><td>114</td><td>111</td><td>108</td><td>105</td><td>102</td><td>99</td><td>96</td></tr>
<tr><th>HTML color code</th><td><tt>#8a0075<tt></td><td><tt>#8d0072<tt></td><td><tt>#90006f<tt></td><td><tt>#93006c<tt></td><td><tt>#960069<tt></td><td><tt>#990066<tt></td><td><tt>#9c0063<tt></td><td><tt>#9f0060<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#8a0075;'></td><td style='background-color:#8d0072;'></td><td style='background-color:#90006f;'></td><td style='background-color:#93006c;'></td><td style='background-color:#960069;'></td><td style='background-color:#990066;'></td><td style='background-color:#9c0063;'></td><td style='background-color:#9f0060;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>224</th><th>225</th><th>226</th><th>227</th><th>228</th><th>229</th><th>230</th><th>231</th></tr>
<tr><th>Red</th><td>162</td><td>165</td><td>168</td><td>171</td><td>174</td><td>177</td><td>180</td><td>183</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>93</td><td>90</td><td>87</td><td>84</td><td>81</td><td>78</td><td>75</td><td>72</td></tr>
<tr><th>HTML color code</th><td><tt>#a2005d<tt></td><td><tt>#a5005a<tt></td><td><tt>#a80057<tt></td><td><tt>#ab0054<tt></td><td><tt>#ae0051<tt></td><td><tt>#b1004e<tt></td><td><tt>#b4004b<tt></td><td><tt>#b70048<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#a2005d;'></td><td style='background-color:#a5005a;'></td><td style='background-color:#a80057;'></td><td style='background-color:#ab0054;'></td><td style='background-color:#ae0051;'></td><td style='background-color:#b1004e;'></td><td style='background-color:#b4004b;'></td><td style='background-color:#b70048;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>232</th><th>233</th><th>234</th><th>235</th><th>236</th><th>237</th><th>238</th><th>239</th></tr>
<tr><th>Red</th><td>186</td><td>189</td><td>192</td><td>195</td><td>198</td><td>201</td><td>204</td><td>207</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>69</td><td>66</td><td>63</td><td>60</td><td>57</td><td>54</td><td>51</td><td>48</td></tr>
<tr><th>HTML color code</th><td><tt>#ba0045<tt></td><td><tt>#bd0042<tt></td><td><tt>#c0003f<tt></td><td><tt>#c3003c<tt></td><td><tt>#c60039<tt></td><td><tt>#c90036<tt></td><td><tt>#cc0033<tt></td><td><tt>#cf0030<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#ba0045;'></td><td style='background-color:#bd0042;'></td><td style='background-color:#c0003f;'></td><td style='background-color:#c3003c;'></td><td style='background-color:#c60039;'></td><td style='background-color:#c90036;'></td><td style='background-color:#cc0033;'></td><td style='background-color:#cf0030;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>240</th><th>241</th><th>242</th><th>243</th><th>244</th><th>245</th><th>246</th><th>247</th></tr>
<tr><th>Red</th><td>210</td><td>213</td><td>216</td><td>219</td><td>222</td><td>225</td><td>228</td><td>231</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>45</td><td>42</td><td>39</td><td>36</td><td>33</td><td>30</td><td>27</td><td>24</td></tr>
<tr><th>HTML color code</th><td><tt>#d2002d<tt></td><td><tt>#d5002a<tt></td><td><tt>#d80027<tt></td><td><tt>#db0024<tt></td><td><tt>#de0021<tt></td><td><tt>#e1001e<tt></td><td><tt>#e4001b<tt></td><td><tt>#e70018<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#d2002d;'></td><td style='background-color:#d5002a;'></td><td style='background-color:#d80027;'></td><td style='background-color:#db0024;'></td><td style='background-color:#de0021;'></td><td style='background-color:#e1001e;'></td><td style='background-color:#e4001b;'></td><td style='background-color:#e70018;'></td></tr>
</tbody>
<tbody>
<tr><th>position</th><th>248</th><th>249</th><th>250</th><th>251</th><th>252</th><th>253</th><th>254</th><th>255</th></tr>
<tr><th>Red</th><td>234</td><td>237</td><td>240</td><td>243</td><td>246</td><td>249</td><td>252</td><td>255</td></tr>
<tr><th>Green</th><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr><th>Blue</th><td>21</td><td>18</td><td>15</td><td>12</td><td>9</td><td>6</td><td>3</td><td>0</td></tr>
<tr><th>HTML color code</th><td><tt>#ea0015<tt></td><td><tt>#ed0012<tt></td><td><tt>#f0000f<tt></td><td><tt>#f3000c<tt></td><td><tt>#f60009<tt></td><td><tt>#f90006<tt></td><td><tt>#fc0003<tt></td><td><tt>#ff0000<tt></td></tr>
<tr><th>HTML color</th><td style='background-color:#ea0015;'></td><td style='background-color:#ed0012;'></td><td style='background-color:#f0000f;'></td><td style='background-color:#f3000c;'></td><td style='background-color:#f60009;'></td><td style='background-color:#f90006;'></td><td style='background-color:#fc0003;'></td><td style='background-color:#ff0000;'></td></tr>
</tbody>
</table>
