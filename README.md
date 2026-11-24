# Roblox Donation Game Template (No Premium Required)

A simple, ready-to-use Roblox template that lets players create their own donation place using **Developer Products**. This allows anyone to donate multiple times without needing Roblox Premium.

## Step 1: Create and Publish Your Place
1. Open **Roblox Studio** and create a new **Baseplate** (or any template).
2. Go to **File** -> **Publish to Roblox**.
3. Name your game `"Donate to [YourUsername]"` (for example, `"Donate to Player123"`) and click **Create**.

## Step 2: Enable Sales in Settings
1. In Roblox Studio, click **Game Settings** at the top.
2. Go to the **Security** tab.
3. Turn ON **Allow Third Party Sales** and click **Save**.

## Step 3: Create Your Developer Products
1. Go to the [Roblox Creator Dashboard](https://roblox.com).
2. Click on your new donation game.
3. In the left menu, go to **Monetization** -> **Developer Products**.
4. Click **Create a Developer Product**.
5. Set the name (e.g., `1 Robux`), upload your icon, and set the **Price in Robux** to `1`.
6. Repeat this for all amounts: **1, 2, 3, 4, 5, 6, 7, 8, 9, 10, and 100**.
7. Copy the **Product ID** for each one (click the three dots `...` on the product -> **Copy Asset ID**).

## Step 4: Add the Client Script (UI & Buttons)
1. In Roblox Studio, open the **Explorer** window on the right.
2. Find **StarterPlayer** and expand it.
3. Right-click on **StarterPlayerScripts** -> **Insert Object** -> **LocalScript**.
4. Paste the following code into the `LocalScript`:

```lua
local MarketplaceService = game:GetService("MarketplaceService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- REPLACE THE ZEROS WITH YOUR DEVELOPER PRODUCT IDs
local donations = {
	{amount = "1", id = 0000000},
	{amount = "2", id = 0000000},
	{amount = "3", id = 0000000},
	{amount = "4", id = 0000000},
	{amount = "5", id = 0000000},
	{amount = "6", id = 0000000},
	{amount = "7", id = 0000000},
	{amount = "8", id = 0000000},
	{amount = "9", id = 0000000},
	{amount = "10", id = 0000000},
	{amount = "100", id = 0000000}
}

-- Create the UI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DonateMenu"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 350, 0, 270)
frame.Position = UDim2.new(0.5, -175, 0.5, -135)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
frame.BorderSizePixel = 0
frame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = frame

local grid = Instance.new("UIGridLayout")
grid.CellSize = UDim2.new(0, 75, 0, 50)
grid.CellPadding = UDim2.new(0, 10, 0, 10)
grid.HorizontalAlignment = Enum.HorizontalAlignment.Center
grid.VerticalAlignment = Enum.VerticalAlignment.Center
grid.Parent = frame

-- Create buttons from the list
for _, item in ipairs(donations) do
	local btn = Instance.new("TextButton")
	btn.Text = item.amount .. " R\$"
	btn.BackgroundColor3 = Color3.fromRGB(0, 170, 100)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 18
	btn.Parent = frame

	local btnCorner = Instance.new("UICorner")
	btnCorner.CornerRadius = UDim.new(0, 8)
	btnCorner.Parent = btn

	btn.MouseButton1Click:Connect(function()
		if item.id ~= 0 then
			MarketplaceService:PromptProductPurchase(player, item.id)
		else
			warn("No Product ID set for " .. item.amount .. " Robux")
		end
	end)
end
```

5. Replace all the `0000000` numbers with your own Developer Product IDs.

## Step 5: Add the Server Script (Fixes Repeat Donations)
1. In the **Explorer** window, find **ServerScriptService**.
2. Right-click on **ServerScriptService** -> **Insert Object** -> **Script**.
3. Paste this code inside (this confirms purchases so players can donate the same amount multiple times):

```lua
local MarketplaceService = game:GetService("MarketplaceService")

MarketplaceService.ProcessReceipt = function(receiptInfo)
	return Enum.ProductPurchaseDecision.PurchaseGranted
end
```

## Step 6: Publish and Share
1. Go to **File** -> **Publish to Roblox** to save your changes.
2. Make sure your game is set to **Public** in the Creator Dashboard so anyone can join and donate to you!
