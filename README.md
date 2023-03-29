MessageBox("GSC PROXY", "The script was injected successfully.")
function FChat(txt)
  p = {}
  p[0] = "OnTextOverlay"
  p[1] = txt
  p.netid = -1
  SendVarlist(p)
end

FChat("`0[`4EXECUTED`0] `9Grow Script Community PROXY")
tax = 0
wls = 0
play = 0
cvbgl = 0
totalbet = 0
function printa(v)
  if v[0] == "OnConsoleMessage" and v[1]:find("Unknown command.") then
    p = {}
    p[0] = "OnConsoleMessage"
    p[1] = "`4Unknown command. `2Enter `0/menu ``to see command list!"
    p.netid = -1
    SendVarlist(p)
    return true
  elseif v[0] == "OnConsoleMessage" and v[1]:find("Spam detected") then
    FChat("`9Slowdown the chat. Spam detected.")
    return true
  elseif v[0] == "OnConsoleMessage" and v[1]:find("Collected ") then
    if v[1]:find("Diamond Lock") then
      play = tonumber(v[1]:match("(%d+) Diamond Lock")) * 100
      countTax(play * 2)
      msg = v[1]
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = msg
      p.netid = -1
      SendVarlist(p)
      return true
    elseif v[1]:find("Blue Gem Lock") then
      play = tonumber(v[1]:match("(%d+) Blue Gem Lock")) * 10000
      countTax(play * 2)
      msg = v[1]
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = msg
      p.netid = -1
      SendVarlist(p)
      return true
    elseif v[1]:find("World Lock") then
      play = tonumber(v[1]:match("(%d+) World Lock"))
      countTax(play * 2)
      msg = v[1]
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = msg
      p.netid = -1
      SendVarlist(p)
      return true
    else
      find = v[1]:find("`5**")
      msg = "`0[`9GSC PROXY`0] " .. v[1]:sub(find)
      p = {}
      p[0] = "OnConsoleMessage"
      p[1] = msg
      p.netid = -1
      SendVarlist(p)
    end
  elseif v[0] == "OnSDBroadcast" then
    log("Blocked SBD")
    return true
  elseif v[0] == "OnConsoleMessage" then
    msg = "`0[`9GSC PROXY`0] " .. v[1]
    p = {}
    p[0] = "OnConsoleMessage"
    p[1] = msg
    p.netid = -1
    SendVarlist(p)
    return true
  elseif v[0] == "OnTalkBubble" and v[3] ~= -1 and v[2]:find("spun the wheel and got") then
    if v[2]:find("``6>``") then
      p = {}
      p[0] = "OnTalkBubble"
      p[1] = v[1]
      p[2] = "`0[`4FAKE`0] " .. v[2]
      p[3] = -1
      p.netid = -1
      SendVarlist(p)
    elseif v[2]:find("``!]``") then
      p = {}
      p[0] = "OnTalkBubble"
      p[1] = v[1]
      p[2] = "`0[`2REAL`0] " .. v[2]
      p[3] = -1
      p.netid = -1
      SendVarlist(p)
      s2 = v[2]:find("`` spun") - 1
      s1 = v[2]:sub(4, s2)
      jml1 = v[2]:find("got ") + 6
      jml2 = v[2]:find("``!") - 1
      jml = v[2]:sub(jml1, jml2)
      for _, player in pairs(GetPlayers()) do
        if player.netid == v[1] then
          cn = {}
          cn[0] = "OnNameChanged"
          cn[1] = s1 .. " [" .. jml .. "]"
          cn.netid = player.netid
          SendVarlist(cn)
        end
      end
    else
      p = {}
      p[0] = "OnTalkBubble"
      p[1] = v[1]
      p[2] = "`0[`4FAKE`0] " .. v[2]
      p[3] = -1
      p.netid = -1
      SendVarlist(p)
    end
    return true
  end
  return false
end

AddCallback(1, "OnVarlist", printa)

function menuDialog()
  menuVarList = {}
  menuVarList[0] = "OnDialogRequest"
  menuVarList[1] = [[
set_default_color|`o
add_label_with_icon|big|`9GSC Proxy Command Menu``|left|32
add_spacer|small|
add_textbox|`0~> `9/mode `0(Set Count Gems Position)|left|
add_textbox|`0~> `9/setpos1 `0(Set Position Count Gems)|left|
add_textbox|`0~> `9/setpos2 `0(Set Position Count Gems)|left|
add_textbox|`0~> `9/setwin1 `0(Set Position Display Box)|left|
add_textbox|`0~> `9/setwin2 `0(Set Position Display Box)|left|
add_textbox|`0~> `9/sethost `0(Set Position Host)|left|
add_spacer|small|
add_textbox|`8-------------- Play Menu|left|
add_textbox|`0~> `9/start `0(Collect WLS from two pos)|left|
add_textbox|`0~> `9/c `0(Count Gems On Position 1 and 2)|left|
add_textbox|`0~> `9/pos `0(Teleport to Host Position)|left|
add_textbox|`0~> `9/res `0(Collect Gems from two pos)|left|
add_textbox|`0~> `9/w1 `0(Teleport to Display Box pos 1)|left|
add_textbox|`0~> `9/w2 `0(Teleport to Display Box pos 2)|left|
add_textbox|`0~> `9/w1d `0(Teleport to Display Box pos 1 and drop Wls (eat tax))|left|
add_textbox|`0~> `9/w2d `0(Teleport to Display Box pos 2 and drop wls (eat tax))|left|
add_textbox|`0~> `9/relog `0(Relog world)|left|
add_spacer|small|
add_textbox|`8-------------- Drop Menu|left|
add_textbox|`0~> `9/wd <amount> `0(World Lock Drop)|left|
add_textbox|`0~> `9/d <amount> `0(Diamond Lock Drop)|left|
add_textbox|`0~> `9/b <amount> `0(Blue Gem Lock Drop)|left|
add_textbox|`0~> `9/cd <amount> `0(Custom Drop)|left|
add_textbox|`0~> `9/phone `0(Open Telephone menu (Fast Convert BGL))|left|
add_spacer|small|
add_textbox|`8-------------- Calculator (Optional)|left|
add_textbox|`0~> `9/tax `0(Set Tax)|left|
add_textbox|`0</Naiko#2017>|left|
end_dialog|Cancel|Host now!|
add_quick_exit]]
  menuVarList.netid = -1
  SendVarlist(menuVarList)
end

function drop(id, amt)
  SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. id .. "|\nitem_count|" .. amt)
  Sleep(180)
end

function findItem(id)
  for _, itm in pairs(GetInventory()) do
    if itm.id == id then
      return itm.count
    end
  end
  return 0
end

function startBET()
  FindPath(poswin1x, poswin1y)
  Sleep(500)
  FindPath(poswin2x, poswin2y)
  Sleep(500)
  FindPath(poshostx, poshosty)
  SendPacket(2, "action|input\ntext|Gas")
end

function relogThread()
  FChat("Relog...")
  latest_world = GetLocal().world
  SendPacket(3, "action|join_request\nname|" .. latest_world .. "\ninvitedWorld|0")
end

function countTax(amount)
  if tonumber(tax) < 10 then
    taxs = tonumber(tax) / 100
    total = math.floor(amount - (amount * taxs))
    FChat("`9" .. tostring(total) .. " WLS `2" .. tostring(math.floor(amount * taxs)) .. " WLS TAX")
    return total
  else
    total = math.floor(amount - (amount / tonumber(tax)))
    FChat("`9" .. tostring(total) .. " WLS `2" .. tostring(math.floor(amount / tax)) .. " WLS TAX")
    return total
  end
end

function checkWinner(count1, count2)
  if count1 == count2 then
    SendPacket(2, "action|input\ntext|(gems) `2" .. count1 .. " `0[TIE] `2" .. count2 .. " (gems)")
  elseif count1 < count2 then
    SendPacket(2, "action|input\ntext|`4[LOSE] (gems) `a" .. count1 .. " `0- `2" .. count2 .. " (gems) `0[WIN]")
  elseif count1 > count2 then
    SendPacket(2, "action|input\ntext|`0[WIN] (gems) `2" .. count1 .. " `0- `a" .. count2 .. " (gems) `4[LOSE]")
  end
end

function customDrop()
  pecahan = { 10000, 100, 1 }
  bgl = 0
  dl = 0
  wl = 0
  for i = 1, 3 do
    while wls >= pecahan[i] do
      if pecahan[i] == pecahan[1] then
        bgl = bgl + 1
        wls = wls - 10000
      elseif pecahan[i] == pecahan[2] then
        dl = dl + 1
        wls = wls - 100
      elseif pecahan[i] == pecahan[3] then
        wl = wl + 1
        wls = wls - 1
      end
    end
  end
  itemamt = { bgl, dl, wl }
  itemid = { 7188, 1796, 242 }
  if findItem(1796) < dl then
    local pkt = {
        type = 10,
        int_data = 7188,
    }
    SendPacketRaw(pkt)
  elseif findItem(242) < wl then
    local pkt = {
      type = 10,
      int_data = 1796,
    }
    SendPacketRaw(pkt)
  end
  Sleep(200)
  if bgl > 0 then
    drop(itemid[1], itemamt[1])
    if dl > 0 then
      drop(itemid[2], itemamt[2])
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    end
  else
    if dl > 0 then
      drop(itemid[2], itemamt[2])
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    else
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    end
  end
  log(" Dropped `1" .. bgl .. " BGL " .. dl .. " DLS " .. wl .. " WLS")
end

function customDropWin()
  pecahan = { 10000, 100, 1 }
  bgl = 0
  dl = 0
  wl = 0
  for i = 1, 3 do
    while total >= pecahan[i] do
      if pecahan[i] == pecahan[1] then
        bgl = bgl + 1
        total = total - 10000
      elseif pecahan[i] == pecahan[2] then
        dl = dl + 1
        total = total - 100
      elseif pecahan[i] == pecahan[3] then
        wl = wl + 1
        total = total - 1
      end
    end
  end
  itemamt = { bgl, dl, wl }
  itemid = { 7188, 1796, 242 }
  if findItem(1796) < dl then
    local pkt = {
        type = 10,
        int_data = 7188,
    }
    SendPacketRaw(pkt)
  elseif findItem(242) < wl then
    local pkt = {
      type = 10,
      int_data = 1796,
    }
    SendPacketRaw(pkt)
  end
  Sleep(200)
  if bgl > 0 then
    drop(itemid[1], itemamt[1])
    if dl > 0 then
      drop(itemid[2], itemamt[2])
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    end
  else
    if dl > 0 then
      drop(itemid[2], itemamt[2])
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    else
      if wl > 0 then
        drop(itemid[3], itemamt[3])
      end
    end
  end
  log(" Dropped `1" .. bgl .. " BGL " .. dl .. " DLS " .. wl .. " WLS")
end

function place(id, x, y)
  pkt = {}
  pkt.type = 3
  pkt.value = id
  pkt.px = math.floor(GetLocal().pos_x / 32 + x)
  pkt.py = math.floor(GetLocal().pos_y / 32 + y)
  pkt.x = GetLocal().pos_x
  pkt.y = GetLocal().pos_y
  SendPacketRaw(false, pkt)
end

function reset()
  if Mode == "HORIZONTAL" then
    FindPath(pos1x1, pos1y1)
    FindPath(pos1x2, pos1y1)
    FindPath(pos1x3, pos1y1)
    Sleep(800)
    FindPath(pos2x1, pos2y1)
    FindPath(pos2x2, pos2y1)
    FindPath(pos2x3, pos2y1)
    Sleep(800)
    FindPath(poshostx, poshosty)
  elseif Mode == "VERTIKAL" then
    FindPath(pos1x1, pos1y1)
    FindPath(pos1x1, pos1y2)
    FindPath(pos1x1, pos1y3)
    Sleep(800)
    FindPath(pos2x1, pos2y1)
    FindPath(pos2x1, pos2y2)
    FindPath(pos2x1, pos2y3)
    Sleep(800)
    FindPath(poshostx, poshosty)
  end
end

AddCallback(0, "OnPacket", function(type, packet)
  if packet:find("/setpos1") then
    if Mode == "VERTIKAL" then
      pos1x1 = math.floor(GetLocal().pos_x / 32)
      pos1y1 = math.floor(GetLocal().pos_y / 32)
      pos1y2 = math.floor(GetLocal().pos_y / 32) - 1
      pos1y3 = math.floor(GetLocal().pos_y / 32) - 2
    elseif Mode == "HORIZONTAL" then
      pos1y1 = math.floor(GetLocal().pos_y / 32)
      pos1x1 = math.floor(GetLocal().pos_x / 32) - 1
      pos1x2 = math.floor(GetLocal().pos_x / 32)
      pos1x3 = math.floor(GetLocal().pos_x / 32) + 1
    end
    FChat("`9[GSC] Pos 1 configured")
    return true
  elseif packet:find("/setpos2") then
    if Mode == "VERTIKAL" then
      pos2x1 = math.floor(GetLocal().pos_x / 32)
      pos2y1 = math.floor(GetLocal().pos_y / 32)
      pos2y2 = math.floor(GetLocal().pos_y / 32) - 1
      pos2y3 = math.floor(GetLocal().pos_y / 32) - 2
      FChat("`9[GSC] Pos 2 configured")
      return true
    elseif Mode == "HORIZONTAL" then
      pos2y1 = math.floor(GetLocal().pos_y / 32)
      pos2x1 = math.floor(GetLocal().pos_x / 32) - 1
      pos2x2 = math.floor(GetLocal().pos_x / 32)
      pos2x3 = math.floor(GetLocal().pos_x / 32) + 1
      FChat("`9[GSC] Pos 2 configured")
      return true
    end
  elseif packet:find("/res") then
    RunThread(reset)
  elseif packet:find("/start") then
    RunThread(startBET)
    return true
  elseif packet:find("/setwin1") then
    poswin1x = math.floor(GetLocal().pos_x / 32)
    poswin1y = math.floor(GetLocal().pos_y / 32)
    FChat("`9[GSC] Pos Win 1 configured")
    return true
  elseif packet:find("/setwin2") then
    poswin2x = math.floor(GetLocal().pos_x / 32)
    poswin2y = math.floor(GetLocal().pos_y / 32)
    FChat("`9[GSC] Pos Win 2 configured")
    return true
  elseif packet:find("/sethost") then
    poshostx = math.floor(GetLocal().pos_x / 32)
    poshosty = math.floor(GetLocal().pos_y / 32)
    FChat("`9[GSC] Pos Host configured")
    return true
  elseif packet:find("/pos") then
    FindPath(poshostx, poshosty)
    return true
  elseif packet:find("/w1d") then
    FindPath(poswin1x, poswin1y)
    RunThread(customDropWin)
    return true
  elseif packet:find("/w2d") then
    FindPath(poswin2x, poswin2y)
    RunThread(customDropWin)
    return true
  elseif packet:find("/w1") then
    -- log("Teleport to win 1")
    FindPath(poswin1x, poswin1y)
    return true
  elseif packet:find("/w2") then
    -- log("Teleport to win 2")
    FindPath(poswin2x, poswin2y)
    return true
  elseif packet:find("/wd (%d+)") then
    wls = tonumber(packet:match("/wd (%d+)"))
    if findItem(242) < wls then
      local pkt = {
        type = 10,
        int_data = 1796,
      }
      SendPacketRaw(pkt)     
    end
    SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. tostring(242) .. "|\nitem_count|" .. wls)
    log("`0[`9GSC PROXY`0] Dropped `1" .. wls .. " WLS")
    return true
  elseif packet:find("/d (%d+)") then
    wls = tonumber(packet:match("/d (%d+)"))
    if findItem(1796) < (wls*100) then
      local pkt = {
        type = 10,
        int_data = 7188,
      }
      SendPacketRaw(pkt)     
    end
    SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. tostring(1796) .. "|\nitem_count|" .. wls)
    log("`0[`9GSC PROXY`0] Dropped `1" .. wls .. " DLS")
    return true
  elseif packet:find("/b (%d+)") then
    wls = tonumber(packet:match("/b (%d+)"))
    if findItem(7188) < (wls*100) then
        log("You don't have that much bgls")
        return true
    end
    SendPacket(2, "action|dialog_return\ndialog_name|drop\nitem_drop|" .. tostring(7188) .. "|\nitem_count|" .. wls)
    log("`0[`9GSC PROXY`0] Dropped `1" .. wls .. " BGL")
    return true
  elseif packet:find("/cd (%d+)") then
    wls = tonumber(packet:match("/cd (%d+)"))
    RunThread(customDrop)
    return true
  elseif packet:find("tax_a_m_o_u_n_t") then
    tax = packet:match("tax_a_m_o_u_n_t|(%d+)")
    FChat("`9Tax has been set `0" .. tax .. "%")
    return true
  elseif packet:find("/tax") then
    menuVarList = {}
    menuVarList[0] = "OnDialogRequest"
    menuVarList[1] = [[
set_default_color|`o
add_label_with_icon|big|`9Set Tax Amount``|left|32
add_spacer|small|
text_scaling_string|iprogramtext
add_spacer|small|
add_textbox|Tax:|left|
add_text_input|tax_a_m_o_u_n_t||]]..tax..[[|3|
add_spacer|small|
end_dialog|growscan_edit||OK|
add_quick_exit]]
    menuVarList.netid = -1
    SendVarlist(menuVarList)
    return true
  elseif packet:find("/relog") then
    RunThread(relogThread)
    return true
  elseif packet:find("/c") or packet:find("/C") then
    if Mode == "VERTIKAL" then
      count1 = 0
      count2 = 0
      for _, value in pairs(GetObjects()) do
        if value.id == 112 then
          checkY = math.floor(value.pos_y / 32)
          checkX = math.floor(value.pos_x / 32)
          if checkX == pos1x1 then
            if checkY == pos1y1 or checkY == pos1y2 or checkY == pos1y3 then
              count1 = count1 + value.count
            end
          end
          if checkX == pos2x1 then
            if checkY == pos2y1 or checkY == pos2y2 or checkY == pos2y3 then
              count2 = count2 + value.count
            end
          end
        end
      end
      count1 = math.floor(count1)
      count2 = math.floor(count2)
      checkWinner(count1, count2)
    elseif Mode == "HORIZONTAL" then
      count1 = 0
      count2 = 0
      for _, value in pairs(GetObjects()) do
        if value.id == 112 then
          checkY = math.floor(value.pos_y / 32)
          checkX = math.floor(value.pos_x / 32)
          if checkY == pos1y1 then
            if checkX == pos1x1 or checkX == pos1x2 or checkX == pos1x3 then
              count1 = count1 + value.count
            end
          end
          if checkY == pos2y1 then
            if checkX == pos2x1 or checkX == pos2x2 or checkX == pos2x3 then
              count2 = count2 + value.count
            end
          end
        end
      end
      count1 = math.floor(count1)
      count2 = math.floor(count2)
      checkWinner(count1, count2)
    end
    return true
  elseif packet:find("wrench_bgl") then
    if cvbgl == 0 then
      cvbgl = 1
      FChat("`9Fast Conver BGL Mode is `0enabled")
      AddCallback("changebgl", "OnVarlist", function(var)
        if var[0] == "OnDialogRequest" and var[1]:find("end_dialog|telephone") then
          x = var[1]:match("x|(%d+)")
          y = var[1]:match("y|(%d+)")
          SendPacket(2,
            "action|dialog_return\ndialog_name|telephone\nnum|53785|\nx|" .. x ..
            "|\ny|" .. y .. "|\nbuttonClicked|bglconvert\n")
          return true
        end
        return false
      end)
    elseif cvbgl == 1 then
      cvbgl = 0
      RemoveCallback("changebgl")
      FChat("`9Fast Conver BGL Mode is `0disabled")
    end
  elseif packet:find("/phone") then
    menuVarList = {}
    menuVarList[0] = "OnDialogRequest"
    menuVarList[1] = [[
set_default_color|`o
add_label_with_icon|big|`9Wrench Menu``|left|32
add_spacer|small|
text_scaling_string|iprogramtext
add_button_with_icon|wrench_bgl|`9Convert BGL|staticYellowFrame|32||
add_button_with_icon||END_LIST|noflags|0||
add_spacer|small|
end_dialog|WrenchShortcut|Cancel|
add_quick_exit]]
    menuVarList.netid = -1
    SendVarlist(menuVarList)
    return true
  elseif packet:find("mode_vertikal") then
    Mode = "VERTIKAL"
    FChat("`9Mode set to `0Vertical")
    return true
  elseif packet:find("mode_horizontal") then
    Mode = "HORIZONTAL"
    FChat("`9Mode set to `0Horizontal")
    return true
  elseif packet:find("/mode") then
    menuVarList = {}
    menuVarList[0] = "OnDialogRequest"
    menuVarList[1] = [[
set_default_color|`o
add_label_with_icon|big|`9Select Mode``|left|32
add_spacer|small|
text_scaling_string|iprogramtext
add_button_with_icon|mode_vertikal|`9Vertikal|staticYellowFrame|646||
add_button_with_icon|mode_horizontal|`9Horizontal|staticYellowFrame|644||
add_button_with_icon||END_LIST|noflags|0||
add_spacer|small|
end_dialog|WrenchShortcut|Cancel|
add_quick_exit]]
    menuVarList.netid = -1
    SendVarlist(menuVarList)
    return true
  elseif packet:find("/menu") then
    menuDialog()
    return true
  end
  return false
end)

menuDialog()
