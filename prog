local m = peripheral.find("monitor")
m.setBackgroundColor(colors.black)
m.clear()
local DEGREE = math.pi/180
local YCORRECTION = 2/3
m.setTextScale(0.5)
m.setCursorPos(1,1)
local size = {m.getSize()}
local maxAge = 0
if size[1] > size[2] then
	maxAge = size[1]/2
else
	maxAge = size[2]/2
end
maxAge = math.floor(maxAge)
local Effect = {}
function Effect:new(x, y)
	local c = 2^math.floor(math.random(1,14))
	local n = 30
	local ndeg = (360/n)
	o = {}
	o.color = c
	o.x = x
	o.y = y
	o.age = 0
	local data = {}
	for i = 1 , n do
		data[i] = {}
		data[i].x = x
		data[i].y = y
		data[i].xincr = math.cos(DEGREE*i*ndeg)
		data[i].yincr = YCORRECTION*math.sin(DEGREE*i*ndeg)
	end
	o.data = data
	self.__index = self
	return setmetatable(o, self)
end
function Effect:Animate()
	if self.age < maxAge then
		for i = 1,#self.data do
			m.setBackgroundColor(colors.black)
			m.setCursorPos(self.data[i]["x"], self.data[i]["y"])
			m.write(" ")
			self.data[i]["x"] = self.data[i]["x"] + self.data[i]["xincr"]
			self.data[i]["y"] = self.data[i]["y"] + self.data[i]["yincr"]
			m.setCursorPos(self.data[i]["x"], self.data[i]["y"])
			m.setBackgroundColor(self.color)
			m.write(" ")
		end
		self.age = self.age + 1
	elseif self.age == maxAge then
		for i = 1,#self.data do
			m.setBackgroundColor(colors.black)
			m.setCursorPos(self.data[i]["x"], self.data[i]["y"])
			m.write(" ")
		end
		self.age = maxAge + 1
	end
end
--Parent code
local Parent = {}
function Parent:new()
	newObj = {container = {},containerIndex = 1}
	self.__index = self
	return setmetatable(newObj, self)
end
function Parent:add(x,y)
	self.container[self.containerIndex] = Effect:new(x,y)
	self.containerIndex = self.containerIndex + 1
	if self.containerIndex > 500 then
		self.containerIndex = 1
	end
end
function Parent:update()
	for i=1,#self.container do
		self.container[i]:Animate()
	end
end
local p = Parent:new()
os.startTimer(0.1)
while true do
	local event, par1, par2, par3 = os.pullEvent()
	if event == "monitor_touch" then
		p:add(par2,par3)
	elseif event == "timer" then
		p:update()
		os.startTimer(0.05)
	end
end