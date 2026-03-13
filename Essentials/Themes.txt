local cloneref = (cloneref or clonereference or function(instance: any)
    return instance
end)
local clonefunction = (clonefunction or copyfunction or function(func) 
    return func 
end)

local HttpService: HttpService = cloneref(game:GetService("HttpService"))
local isfolder, isfile, listfiles = isfolder, isfile, listfiles

if typeof(clonefunction) == "function" then
    -- Fix is_____ functions for shitsploits, those functions should never error, only return a boolean.

    local
        isfolder_copy,
        isfile_copy,
        listfiles_copy = clonefunction(isfolder), clonefunction(isfile), clonefunction(listfiles)

    local isfolder_success, isfolder_error = pcall(function()
        return isfolder_copy("test" .. tostring(math.random(1000000, 9999999)))
    end)

    if isfolder_success == false or typeof(isfolder_error) ~= "boolean" then
        isfolder = function(folder)
            local success, data = pcall(isfolder_copy, folder)
            return (if success then data else false)
        end

        isfile = function(file)
            local success, data = pcall(isfile_copy, file)
            return (if success then data else false)
        end

        listfiles = function(folder)
            local success, data = pcall(listfiles_copy, folder)
            return (if success then data else {})
        end
    end
end

local ThemeManager = {}
do
    local ThemeFields = { "FontColor", "MainColor", "AccentColor", "BackgroundColor", "OutlineColor" }
    ThemeManager.Folder = "ObsidianLibSettings"
    -- if not isfolder(ThemeManager.Folder) then makefolder(ThemeManager.Folder) end

    ThemeManager.Library = nil
    ThemeManager.AppliedToTab = false
    ThemeManager.BuiltInThemes = {
        ["Default"] = {
            1,
            { FontColor = "ffffff", MainColor = "191919", AccentColor = "7d55ff", BackgroundColor = "0f0f0f", OutlineColor = "282828" },
        },
        ["BBot"] = {
            2,
            { FontColor = "ffffff", MainColor = "1e1e1e", AccentColor = "7e48a3", BackgroundColor = "232323", OutlineColor = "141414" },
        },
        ["Fatality"] = {
            3,
            { FontColor = "ffffff", MainColor = "1e1842", AccentColor = "c50754", BackgroundColor = "191335", OutlineColor = "3c355d" },
        },
        ["Jester"] = {
            4,
            { FontColor = "ffffff", MainColor = "242424", AccentColor = "db4467", BackgroundColor = "1c1c1c", OutlineColor = "373737" },
        },
        ["Mint"] = {
            5,
            { FontColor = "ffffff", MainColor = "242424", AccentColor = "3db488", BackgroundColor = "1c1c1c", OutlineColor = "373737" },
        },
        ["Tokyo Night"] = {
            6,
            { FontColor = "ffffff", MainColor = "191925", AccentColor = "6759b3", BackgroundColor = "16161f", OutlineColor = "323232" },
        },
        ["Ubuntu"] = {
            7,
            { FontColor = "ffffff", MainColor = "3e3e3e", AccentColor = "e2581e", BackgroundColor = "323232", OutlineColor = "191919" },
        },
        ["Quartz"] = {
            8,
            { FontColor = "ffffff", MainColor = "232330", AccentColor = "426e87", BackgroundColor = "1d1b26", OutlineColor = "27232f" },
        },
        ["Nord"] = {
            9,
            { FontColor = "eceff4", MainColor = "3b4252", AccentColor = "88c0d0", BackgroundColor = "2e3440", OutlineColor = "4c566a" },
        },
        ["Dracula"] = {
            10,
            { FontColor = "f8f8f2", MainColor = "44475a", AccentColor = "ff79c6", BackgroundColor = "282a36", OutlineColor = "6272a4" },
        },
        ["Monokai"] = {
            11,
            { FontColor = "f8f8f2", MainColor = "272822", AccentColor = "f92672", BackgroundColor = "1e1f1c", OutlineColor = "49483e" },
        },
        ["Gruvbox"] = {
            12,
            { FontColor = "ebdbb2", MainColor = "3c3836", AccentColor = "fb4934", BackgroundColor = "282828", OutlineColor = "504945" },
        },
        ["Solarized"] = {
            13,
            { FontColor = "839496", MainColor = "073642", AccentColor = "cb4b16", BackgroundColor = "002b36", OutlineColor = "586e75" },
        },
        ["Catppuccin"] = {
            14,
            { FontColor = "d9e0ee", MainColor = "302d41", AccentColor = "f5c2e7", BackgroundColor = "1e1e2e", OutlineColor = "575268" },
        },
        ["One Dark"] = {
            15,
            { FontColor = "abb2bf", MainColor = "282c34", AccentColor = "c678dd", BackgroundColor = "21252b", OutlineColor = "5c6370" },
        },
        ["Cyberpunk"] = {
            16,
            { FontColor = "f9f9f9", MainColor = "262335", AccentColor = "00ff9f", BackgroundColor = "1a1a2e", OutlineColor = "413c5e" },
        },
        ["Oceanic Next"] = {
            17,
            { FontColor = "d8dee9", MainColor = "1b2b34", AccentColor = "6699cc", BackgroundColor = "16232a", OutlineColor = "343d46" },
        },
        ["Material"] = {
            18,
            { FontColor = "eeffff", MainColor = "212121", AccentColor = "82aaff", BackgroundColor = "151515", OutlineColor = "424242" },
        },

        -- Linux / Distro Themes
        ["Arch Linux"] = {
            19,
            { FontColor = "d8dee9", MainColor = "1b2135", AccentColor = "1793d1", BackgroundColor = "0f1420", OutlineColor = "2a3550" },
        },
        ["Kali Linux"] = {
            20,
            { FontColor = "ffffff", MainColor = "1a1a2e", AccentColor = "557ac1", BackgroundColor = "0d0d1a", OutlineColor = "2a2a4a" },
        },
        ["Fedora"] = {
            21,
            { FontColor = "ffffff", MainColor = "2a2a2a", AccentColor = "3c6eb4", BackgroundColor = "1c1c1c", OutlineColor = "404040" },
        },
        ["Manjaro"] = {
            22,
            { FontColor = "f0f0f0", MainColor = "1f2225", AccentColor = "35bf5c", BackgroundColor = "171a1d", OutlineColor = "2e3338" },
        },
        ["Pop! OS"] = {
            23,
            { FontColor = "ffffff", MainColor = "2b2b2b", AccentColor = "48b9c7", BackgroundColor = "1c1c1c", OutlineColor = "383838" },
        },
        ["Mint Linux"] = {
            24,
            { FontColor = "e8e8e8", MainColor = "2e3436", AccentColor = "5cb85c", BackgroundColor = "1e2426", OutlineColor = "404040" },
        },
        ["Debian"] = {
            25,
            { FontColor = "f0f0f0", MainColor = "242424", AccentColor = "c1002b", BackgroundColor = "181818", OutlineColor = "3a3a3a" },
        },
        ["Void Linux"] = {
            26,
            { FontColor = "dcdccc", MainColor = "1d2021", AccentColor = "478061", BackgroundColor = "141617", OutlineColor = "2e3132" },
        },

        -- VSCode / Editor Themes
        ["Ayu Dark"] = {
            27,
            { FontColor = "e6e1cf", MainColor = "1f2430", AccentColor = "ffb454", BackgroundColor = "0d1017", OutlineColor = "2a3140" },
        },
        ["Palenight"] = {
            28,
            { FontColor = "a6accd", MainColor = "292d3e", AccentColor = "c792ea", BackgroundColor = "1e2030", OutlineColor = "444267" },
        },
        ["Shades of Purple"] = {
            29,
            { FontColor = "ffffff", MainColor = "2d2b55", AccentColor = "fad000", BackgroundColor = "1e1e3f", OutlineColor = "4d4b7c" },
        },
        ["GitHub Dark"] = {
            30,
            { FontColor = "e6edf3", MainColor = "21262d", AccentColor = "388bfd", BackgroundColor = "161b22", OutlineColor = "30363d" },
        },
        ["Panda"] = {
            31,
            { FontColor = "e6e6e6", MainColor = "292a2b", AccentColor = "19f9d8", BackgroundColor = "1c1c1c", OutlineColor = "404040" },
        },
        ["Night Owl"] = {
            32,
            { FontColor = "d6deeb", MainColor = "011627", AccentColor = "7fdbca", BackgroundColor = "010e1a", OutlineColor = "1d3b53" },
        },
        ["Cobalt2"] = {
            33,
            { FontColor = "ffffff", MainColor = "193549", AccentColor = "ffc600", BackgroundColor = "132738", OutlineColor = "1f4462" },
        },
        ["Horizon"] = {
            34,
            { FontColor = "e8c9a0", MainColor = "1c1e26", AccentColor = "e95678", BackgroundColor = "16181e", OutlineColor = "2a2c37" },
        },
        ["Rosé Pine"] = {
            35,
            { FontColor = "e0def4", MainColor = "26233a", AccentColor = "ebbcba", BackgroundColor = "191724", OutlineColor = "403d52" },
        },
        ["Everforest"] = {
            36,
            { FontColor = "d3c6aa", MainColor = "3c4841", AccentColor = "a7c080", BackgroundColor = "2d353b", OutlineColor = "4a5560" },
        },

        -- Browser / Aesthetic Themes
        ["Arc"] = {
            37,
            { FontColor = "f0f0f5", MainColor = "2a2a3d", AccentColor = "8b79f7", BackgroundColor = "1a1a2e", OutlineColor = "3d3d58" },
        },
        ["Firefox Dark"] = {
            38,
            { FontColor = "f9f9fb", MainColor = "2b2a33", AccentColor = "9058ff", BackgroundColor = "1c1b22", OutlineColor = "42414d" },
        },
        ["Chrome Dark"] = {
            39,
            { FontColor = "e8eaed", MainColor = "292929", AccentColor = "8ab4f8", BackgroundColor = "202124", OutlineColor = "3c4043" },
        },
        ["Brave Browser"] = {
            40,
            { FontColor = "f2f2f2", MainColor = "26272b", AccentColor = "fb7c44", BackgroundColor = "1a1a1e", OutlineColor = "3a3b40" },
        },

        -- Aesthetic / Vibe Themes
        ["Ocean"] = {
            41,
            { FontColor = "cdd6f4", MainColor = "1e3a5f", AccentColor = "00b4d8", BackgroundColor = "0a1628", OutlineColor = "1a4a7a" },
        },
        ["Wave"] = {
            42,
            { FontColor = "e0f7fa", MainColor = "162a3a", AccentColor = "00e5ff", BackgroundColor = "0d1f2d", OutlineColor = "1e3d54" },
        },
        ["Creamy"] = {
            43,
            { FontColor = "3d2e1e", MainColor = "e8dcc8", AccentColor = "c0874f", BackgroundColor = "f5ede0", OutlineColor = "d4c4aa" },
        },
        ["Sakura"] = {
            44,
            { FontColor = "3d1a2a", MainColor = "f5c8d8", AccentColor = "e8779a", BackgroundColor = "fde8f0", OutlineColor = "e8b0c8" },
        },
        ["Forest"] = {
            45,
            { FontColor = "d8e8d0", MainColor = "1a2e1a", AccentColor = "5a9c52", BackgroundColor = "101e10", OutlineColor = "2a4228" },
        },
        ["Sunset"] = {
            46,
            { FontColor = "ffe0b2", MainColor = "2d1a0e", AccentColor = "ff6e40", BackgroundColor = "1a0a04", OutlineColor = "4a2a18" },
        },
        ["Neon"] = {
            47,
            { FontColor = "ffffff", MainColor = "0a0a0a", AccentColor = "ff00ff", BackgroundColor = "050505", OutlineColor = "1a001a" },
        },
        ["Synthwave"] = {
            48,
            { FontColor = "f8f8f2", MainColor = "2a1a4a", AccentColor = "ff2f96", BackgroundColor = "1a0a2e", OutlineColor = "3d2060" },
        },
        ["Coffee"] = {
            49,
            { FontColor = "ede0d4", MainColor = "3b2314", AccentColor = "b5651d", BackgroundColor = "2c1a0e", OutlineColor = "5c3a1e" },
        },
        ["Lavender"] = {
            50,
            { FontColor = "f0eeff", MainColor = "2e2640", AccentColor = "b39ddb", BackgroundColor = "1e1830", OutlineColor = "4a3d66" },
        },
        ["Slate"] = {
            51,
            { FontColor = "e2e8f0", MainColor = "334155", AccentColor = "94a3b8", BackgroundColor = "1e293b", OutlineColor = "475569" },
        },
        ["Ash"] = {
            52,
            { FontColor = "e0e0e0", MainColor = "3a3a3a", AccentColor = "9e9e9e", BackgroundColor = "262626", OutlineColor = "505050" },
        },
        ["Midnight"] = {
            53,
            { FontColor = "c8c8d8", MainColor = "151520", AccentColor = "5555ff", BackgroundColor = "0a0a14", OutlineColor = "252535" },
        },
        ["Blood Moon"] = {
            54,
            { FontColor = "ffe0e0", MainColor = "2a0a0a", AccentColor = "cc0000", BackgroundColor = "1a0505", OutlineColor = "4a1010" },
        },
        ["Obsidian"] = {
            55,
            { FontColor = "e8e8e8", MainColor = "1a1a1a", AccentColor = "705dcf", BackgroundColor = "0d0d0d", OutlineColor = "2a2a2a" },
        },
        ["Latte"] = {
            56,
            { FontColor = "4c4f69", MainColor = "ccd0da", AccentColor = "8839ef", BackgroundColor = "eff1f5", OutlineColor = "bcc0cc" },
        },
        ["Candy"] = {
            57,
            { FontColor = "ffffff", MainColor = "2a1a3a", AccentColor = "ff6eb4", BackgroundColor = "1a0a2a", OutlineColor = "4a2a5a" },
        },
        ["Matrix"] = {
            58,
            { FontColor = "00ff41", MainColor = "0a1a0a", AccentColor = "00cc33", BackgroundColor = "050f05", OutlineColor = "0a2a0a" },
        }
    }

    function ThemeManager:SetLibrary(library)
        self.Library = library
    end

    --// Folders \\--
    function ThemeManager:GetPaths()
        local paths = {}

        local parts = self.Folder:split("/")
        for idx = 1, #parts do
            paths[#paths + 1] = table.concat(parts, "/", 1, idx)
        end

        paths[#paths + 1] = self.Folder .. "/themes"

        return paths
    end

    function ThemeManager:BuildFolderTree()
        local paths = self:GetPaths()

        for i = 1, #paths do
            local str = paths[i]
            if isfolder(str) then
                continue
            end
            makefolder(str)
        end
    end

    function ThemeManager:CheckFolderTree()
        if isfolder(self.Folder) then
            return
        end
        self:BuildFolderTree()

        task.wait(0.1)
    end

    function ThemeManager:SetFolder(folder)
        self.Folder = folder
        self:BuildFolderTree()
    end

    --// Apply, Update theme \\--
    function ThemeManager:ApplyTheme(theme)
        local customThemeData = self:GetCustomTheme(theme)
        local data = customThemeData or self.BuiltInThemes[theme]

        if not data then
            return
        end

        local scheme = data[2]
        for idx, val in pairs(customThemeData or scheme) do
            if idx == "VideoLink" then
                continue
            elseif idx == "FontFace" then
                self.Library:SetFont(Enum.Font[val])

                if self.Library.Options[idx] then
                    self.Library.Options[idx]:SetValue(val)
                end
            else
                self.Library.Scheme[idx] = Color3.fromHex(val)

                if self.Library.Options[idx] then
                    self.Library.Options[idx]:SetValueRGB(Color3.fromHex(val))
                end
            end
        end

        self:ThemeUpdate()
    end

    function ThemeManager:ThemeUpdate()
        for i, field in ThemeFields do
            if self.Library.Options and self.Library.Options[field] then
                self.Library.Scheme[field] = self.Library.Options[field].Value
            end
        end

        self.Library:UpdateColorsUsingRegistry()
    end

    --// Get, Load, Save, Delete, Refresh \\--
    function ThemeManager:GetCustomTheme(file)
        local path = self.Folder .. "/themes/" .. file .. ".json"
        if not isfile(path) then
            return nil
        end

        local data = readfile(path)
        local success, decoded = pcall(HttpService.JSONDecode, HttpService, data)

        if not success then
            return nil
        end

        return decoded
    end

    function ThemeManager:LoadDefault()
        local theme = "Default"
        local content = isfile(self.Folder .. "/themes/default.txt") and readfile(self.Folder .. "/themes/default.txt")

        local isDefault = true
        if content then
            if self.BuiltInThemes[content] then
                theme = content
            elseif self:GetCustomTheme(content) then
                theme = content
                isDefault = false
            end
        elseif self.BuiltInThemes[self.DefaultTheme] then
            theme = self.DefaultTheme
        end

        if isDefault then
            self.Library.Options.ThemeManager_ThemeList:SetValue(theme)
        else
            self:ApplyTheme(theme)
        end
    end

    function ThemeManager:SaveDefault(theme)
        writefile(self.Folder .. "/themes/default.txt", theme)
    end

    function ThemeManager:SetDefaultTheme(theme)
        assert(self.Library, "Must set ThemeManager.Library first!")
        assert(not self.AppliedToTab, "Cannot set default theme after applying ThemeManager to a tab!")

        local FinalTheme = {}
        local LibraryScheme = {}
        for _, field in ThemeFields do
            if typeof(theme[field]) == "Color3" then
                FinalTheme[field] = "#" .. theme[field]:ToHex()
                LibraryScheme[field] = theme[field]

            elseif typeof(theme[field]) == "string" then
                FinalTheme[field] = if theme[field]:sub(1, 1) == "#" then theme[field] else ("#" .. theme[field])
                LibraryScheme[field] = Color3.fromHex(theme[field])

            else
                FinalTheme[field] = ThemeManager.BuiltInThemes["Default"][2][field]
                LibraryScheme[field] = Color3.fromHex(ThemeManager.BuiltInThemes["Default"][2][field])
            end
        end

        if typeof(theme["FontFace"]) == "EnumItem" then
            FinalTheme["FontFace"] = theme["FontFace"].Name
            LibraryScheme["Font"] = Font.fromEnum(theme["FontFace"])

        elseif typeof(theme["FontFace"]) == "string" then
            FinalTheme["FontFace"] = theme["FontFace"]
            LibraryScheme["Font"] = Font.fromEnum(Enum.Font[theme["FontFace"]])

        else
            FinalTheme["FontFace"] = "Code"
            LibraryScheme["Font"] = Font.fromEnum(Enum.Font.Code)
        end

        for _, field in { "RedColor", "DarkColor", "WhiteColor" } do
            LibraryScheme[field] = self.Library.Scheme[field]
        end

        self.Library.Scheme = LibraryScheme
        self.BuiltInThemes["Default"] = { 1, FinalTheme }

        self.Library:UpdateColorsUsingRegistry()
    end

    function ThemeManager:SaveCustomTheme(file)
        if file:gsub(" ", "") == "" then
            self.Library:Notify("Invalid file name for theme (empty)", 3)
            return
        end

        local theme = {}
        for _, field in ThemeFields do
            theme[field] = self.Library.Options[field].Value:ToHex()
        end
        theme["FontFace"] = self.Library.Options["FontFace"].Value

        writefile(self.Folder .. "/themes/" .. file .. ".json", HttpService:JSONEncode(theme))
    end

    function ThemeManager:Delete(name)
        if not name then
            return false, "no config file is selected"
        end

        local file = self.Folder .. "/themes/" .. name .. ".json"
        if not isfile(file) then
            return false, "invalid file"
        end

        local success = pcall(delfile, file)
        if not success then
            return false, "delete file error"
        end

        return true
    end

    function ThemeManager:ReloadCustomThemes()
        local list = listfiles(self.Folder .. "/themes")

        local out = {}
        for i = 1, #list do
            local file = list[i]
            if file:sub(-5) == ".json" then
                -- i hate this but it has to be done ...

                local pos = file:find(".json", 1, true)
                local start = pos

                local char = file:sub(pos, pos)
                while char ~= "/" and char ~= "\\" and char ~= "" do
                    pos = pos - 1
                    char = file:sub(pos, pos)
                end

                if char == "/" or char == "\\" then
                    table.insert(out, file:sub(pos + 1, start - 1))
                end
            end
        end

        return out
    end

    --// GUI \\--
    function ThemeManager:CreateThemeManager(groupbox)
        groupbox
            :AddLabel("Background color")
            :AddColorPicker("BackgroundColor", { Default = self.Library.Scheme.BackgroundColor })
        groupbox:AddLabel("Main color"):AddColorPicker("MainColor", { Default = self.Library.Scheme.MainColor })
        groupbox:AddLabel("Accent color"):AddColorPicker("AccentColor", { Default = self.Library.Scheme.AccentColor })
        groupbox
            :AddLabel("Outline color")
            :AddColorPicker("OutlineColor", { Default = self.Library.Scheme.OutlineColor })
        groupbox:AddLabel("Font color"):AddColorPicker("FontColor", { Default = self.Library.Scheme.FontColor })
        groupbox:AddDropdown("FontFace", {
            Text = "Font Face",
            Default = "Code",
            Values = { "BuilderSans", "Code", "Fantasy", "Gotham", "Jura", "Roboto", "RobotoMono", "SourceSans" },
        })

        local ThemesArray = {}
        for Name, Theme in pairs(self.BuiltInThemes) do
            table.insert(ThemesArray, Name)
        end

        table.sort(ThemesArray, function(a, b)
            return self.BuiltInThemes[a][1] < self.BuiltInThemes[b][1]
        end)

        groupbox:AddDivider()

        groupbox:AddDropdown("ThemeManager_ThemeList", { Text = "Theme list", Values = ThemesArray, Default = 1 })
        groupbox:AddButton("Set as default", function()
            self:SaveDefault(self.Library.Options.ThemeManager_ThemeList.Value)
            self.Library:Notify(
                string.format("Set default theme to %q", self.Library.Options.ThemeManager_ThemeList.Value)
            )
        end)

        self.Library.Options.ThemeManager_ThemeList:OnChanged(function()
            self:ApplyTheme(self.Library.Options.ThemeManager_ThemeList.Value)
        end)

        groupbox:AddDivider()

        groupbox:AddInput("ThemeManager_CustomThemeName", { Text = "Custom theme name" })
        groupbox:AddButton("Create theme", function()
            local name = self.Library.Options.ThemeManager_CustomThemeName.Value

            if name:gsub(" ", "") == "" then
                self.Library:Notify("Invalid theme name (empty)", 2)
                return
            end

            self:SaveCustomTheme(name)

            self.Library:Notify(string.format("Created theme %q", name))
            self.Library.Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
            self.Library.Options.ThemeManager_CustomThemeList:SetValue(nil)
        end)

        groupbox:AddDivider()

        groupbox:AddDropdown(
            "ThemeManager_CustomThemeList",
            { Text = "Custom themes", Values = self:ReloadCustomThemes(), AllowNull = true, Default = 1 }
        )
        groupbox:AddButton("Load theme", function()
            local name = self.Library.Options.ThemeManager_CustomThemeList.Value

            self:ApplyTheme(name)
            self.Library:Notify(string.format("Loaded theme %q", name))
        end)
        groupbox:AddButton("Overwrite theme", function()
            local name = self.Library.Options.ThemeManager_CustomThemeList.Value

            self:SaveCustomTheme(name)
            self.Library:Notify(string.format("Overwrote config %q", name))
        end)
        groupbox:AddButton("Delete theme", function()
            local name = self.Library.Options.ThemeManager_CustomThemeList.Value

            local success, err = self:Delete(name)
            if not success then
                self.Library:Notify("Failed to delete theme: " .. err)
                return
            end

            self.Library:Notify(string.format("Deleted theme %q", name))
            self.Library.Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
            self.Library.Options.ThemeManager_CustomThemeList:SetValue(nil)
        end)
        groupbox:AddButton("Refresh list", function()
            self.Library.Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
            self.Library.Options.ThemeManager_CustomThemeList:SetValue(nil)
        end)
        groupbox:AddButton("Set as default", function()
            if
                self.Library.Options.ThemeManager_CustomThemeList.Value ~= nil
                and self.Library.Options.ThemeManager_CustomThemeList.Value ~= ""
            then
                self:SaveDefault(self.Library.Options.ThemeManager_CustomThemeList.Value)
                self.Library:Notify(
                    string.format("Set default theme to %q", self.Library.Options.ThemeManager_CustomThemeList.Value)
                )
            end
        end)
        groupbox:AddButton("Reset default", function()
            local success = pcall(delfile, self.Folder .. "/themes/default.txt")
            if not success then
                self.Library:Notify("Failed to reset default: delete file error")
                return
            end

            self.Library:Notify("Set default theme to nothing")
            self.Library.Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
            self.Library.Options.ThemeManager_CustomThemeList:SetValue(nil)
        end)

        self:LoadDefault()
        self.AppliedToTab = true

        local function UpdateTheme()
            self:ThemeUpdate()
        end

        self.Library.Options.BackgroundColor:OnChanged(UpdateTheme)
        self.Library.Options.MainColor:OnChanged(UpdateTheme)
        self.Library.Options.AccentColor:OnChanged(UpdateTheme)
        self.Library.Options.OutlineColor:OnChanged(UpdateTheme)
        self.Library.Options.FontColor:OnChanged(UpdateTheme)
        self.Library.Options.FontFace:OnChanged(function(Value)
            self.Library:SetFont(Enum.Font[Value])
            self.Library:UpdateColorsUsingRegistry()
        end)
    end

    function ThemeManager:CreateGroupBox(tab)
        assert(self.Library, "Must set ThemeManager.Library first!")
        return tab:AddLeftGroupbox("Themes", "paintbrush")
    end

    function ThemeManager:ApplyToTab(tab)
        assert(self.Library, "Must set ThemeManager.Library first!")
        local groupbox = self:CreateGroupBox(tab)
        self:CreateThemeManager(groupbox)
    end

    function ThemeManager:ApplyToGroupbox(groupbox)
        assert(self.Library, "Must set ThemeManager.Library first!")
        self:CreateThemeManager(groupbox)
    end

    ThemeManager:BuildFolderTree()
end

getgenv().ObsidianThemeManager = ThemeManager
return ThemeManager
