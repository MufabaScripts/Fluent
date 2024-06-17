Sure! I'll integrate the section creation code into the "Creating Tabs" part and provide a complete example:

# Fluent Library Documentation

This documentation is for the stable release of Fluent Library.

## Booting the Library

To load the Fluent library, use the following code:

```lua
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()
```

## Creating a Window

To create a window using Fluent:

```lua
local Window = Fluent:CreateWindow({
    Title = "Fluent " .. Fluent.Version,
    SubTitle = "by dawid",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when there's no MinimizeKeybind
})
```

## Creating Tabs

To add tabs to the window:

```lua
local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}
```

## Creating Sections

To add Sections to the Tabs:

```lua
local Section = Tabs.Main:AddSection("Section")
```

## Notifying the User

To show notifications to the user:

```lua
Fluent:Notify({
    Title = "Notification",
    Content = "This is a notification",
    SubContent = "SubContent", -- Optional
    Duration = 5 -- Set to nil to make the notification not disappear
})
```

## Creating a Paragraph

To add a paragraph:

```lua
Tabs.Main:AddParagraph({
    Title = "Paragraph",
    Content = "This is a paragraph.\nSecond line!"
})
```

### Refreshing a Paragraph

To refresh an existing paragraph:

```lua
local Paragraph = Tabs.Main:AddParagraph({
    Title = "Initial Title",
    Content = "Initial Content"
})

Paragraph:SetTitle("New Title")
Paragraph:SetDesc("New Content")
```

## Creating a Button

To add a button:

```lua
Tabs.Main:AddButton({
    Title = "Button",
    Description = "Very important button",
    Callback = function()
        -- Action to be performed when button is clicked
    end
})
```

### Button with Dialog

To add a button that opens a dialog:

```lua
Tabs.Main:AddButton({
    Title = "Button with Dialog",
    Description = "Opens a dialog on click",
    Callback = function()
        Window:Dialog({
            Title = "Dialog Title",
            Content = "This is a dialog",
            Buttons = {
                {
                    Title = "Confirm",
                    Callback = function()
                        print("Confirmed the dialog.")
                    end
                },
                {
                    Title = "Cancel",
                    Callback = function()
                        print("Cancelled the dialog.")
                    end
                }
            }
        })
    end
})
```

## Creating a Toggle

To add a toggle switch:

```lua
local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Toggle", Default = false })

Toggle:OnChanged(function()
    print("Toggle changed:", Options.MyToggle.Value)
end)

Options.MyToggle:SetValue(false)
```

## Creating a Slider

To add a slider:

```lua
local Slider = Tabs.Main:AddSlider("Slider", {
    Title = "Slider",
    Description = "This is a slider",
    Default = 2,
    Min = 0,
    Max = 5,
    Rounding = 1,
    Callback = function(Value)
        print("Slider was changed:", Value)
    end
})

Slider:OnChanged(function(Value)
    print("Slider changed:", Value)
end)

Slider:SetValue(3)
```

## Creating a Dropdown Menu

To add a dropdown menu:

```lua
local Dropdown = Tabs.Main:AddDropdown("Dropdown", {
    Title = "Dropdown",
    Values = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine", "ten", "eleven", "twelve", "thirteen", "fourteen"},
    Multi = false,
    Default = 1,
})

Dropdown:SetValue("four")

Dropdown:OnChanged(function(Value)
    print("Dropdown changed:", Value)
end)
```

### Refreshing a Dropdown

To refresh the values in an existing dropdown:

```lua
Dropdown:SetValues({"hello", "one", "two"})
Dropdown:SetValue(nil)
```

## Creating a Multi-Select Dropdown Menu

To add a multi-select dropdown menu:

```lua
local MultiDropdown = Tabs.Main:AddDropdown("MultiDropdown", {
    Title = "Dropdown",
    Description = "You can select multiple values.",
    Values = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine", "ten", "eleven", "twelve", "thirteen", "fourteen"},
    Multi = true,
    Default = {"seven", "twelve"},
})

MultiDropdown:SetValue({
    three = true,
    five = true,
    seven = false
})

MultiDropdown:OnChanged(function(Value)
    local Values = {}
    for Value, State in next, Value do
        table.insert(Values, Value)
    end
    print("Mutlidropdown changed:", table.concat(Values, ", "))
end)
```

## Creating a Color Picker

To add a color picker:

```lua
local Colorpicker = Tabs.Main:AddColorpicker("Colorpicker", {
    Title = "Colorpicker",
    Default = Color3.fromRGB(96, 205, 255)
})

Colorpicker:OnChanged(function()
    print("Colorpicker changed:", Colorpicker.Value)
end)

Colorpicker:SetValueRGB(Color3.fromRGB(0, 255, 140))
```

## Creating a Color Picker with Transparency

To add a color picker that also allows transparency adjustment:

```lua
local TColorpicker = Tabs.Main:AddColorpicker("TransparencyColorpicker", {
    Title = "Colorpicker",
    Description = "but you can change the transparency.",
    Transparency = 0,
    Default = Color3.fromRGB(96, 205, 255)
})

TColorpicker:OnChanged(function()
    print(
        "TColorpicker changed:", TColorpicker.Value,
        "Transparency:", TColorpicker.Transparency
    )
end)
```

## Creating a Keybind

To add a keybind:

```lua
local Keybind = Tabs.Main:AddKeybind("Keybind", {
    Title = "KeyBind",
    Mode = "Toggle", -- Always, Toggle, Hold
    Default = "LeftControl", -- String as the name of the keybind (MB1, MB2 for mouse buttons)

    -- Occurs when the keybind is clicked, Value is `true`/`false`
    Callback = function(Value)
        print("Keybind clicked!", Value)
    end,

    -- Occurs when the keybind itself is changed, `New` is a KeyCode Enum OR a UserInputType Enum
    ChangedCallback = function(New)
        print("Keybind changed!", New)
    end
})

-- OnClick is only fired when you press the keybind and the mode is Toggle
-- Otherwise, you will have to use Keybind:GetState()
Keybind:OnClick(function()
    print("Keybind clicked:", Keybind:GetState())
end)

Keybind:OnChanged(function()
    print("Keybind changed:", Keybind.Value)
end)

task.spawn(function()
    while true do
        wait(1)

        -- example for checking if a keybind is being pressed
        local state = Keybind:GetState()
        if state then
            print("Keybind is being held down")
        end

        if Fluent.Unloaded then break end
    end
end)

Keybind:SetValue("MB2", "Toggle") -- Sets keybind to MB2, mode to Hold
```

## Creating an Input

To add an input box:

```lua
local Input = Tabs.Main:AddInput("Input", {
    Title = "Input",
    Default = "Default",
    Placeholder = "Placeholder",
    Numeric = false, -- Only allows numbers
    Finished = false, -- Only calls callback when you press enter
    Callback = function(Value)
        print("Input changed:", Value)
    end
})

Input:OnChanged(function()
    print("Input updated:", Input.Value)
end)
```

## Addons: SaveManager and InterfaceManager

The Fluent library supports SaveManager and InterfaceManager for managing configurations and interfaces.

```lua
-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- InterfaceManager (Allows you to have an interface management system)

-- Hand the library over to our managers
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

-- Ignore keys that are used by ThemeManager.
-- (we don't want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- You can add indexes of elements the save manager should ignore
SaveManager:SetIgnoreIndexes({})

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs

.Settings)
```

---

Feel free to adjust and expand upon this documentation to suit your needs!
