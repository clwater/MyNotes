# tips


## urlEncode&urlDecode
``` lua
local function urlEncode(s)  
     s = string.gsub(s, "([^%w%.%- ])", function(c) return string.format("%%%02X", string.byte(c)) end)  
    return string.gsub(s, " ", "+")  
end  

local function urlDecode(s)  
    s = string.gsub(s, '%%(%x%x)', function(h) return string.char(tonumber(h, 16)) end)  
    return s  
end  
```

## 去除收尾空格
``` lua
function trim(input)
	return (string.gsub(input, "^%s*(.-)%s*$", "%1"))
end
```