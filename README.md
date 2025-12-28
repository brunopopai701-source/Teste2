 RayfieldOrErr = pcall(function()
    return loadstring(game:HttpGet("https://sirius.menu/rayfield"))()
end)

if not ok or not RayfieldOrErr then
    warn("Falha ao carregar Rayfield:", RayfieldOrErr)
    return
end

local Rayfield = RayfieldOrErr

-- Estado de desbloqueio (fallback manual caso a KeySystem não funcione)
local unlocked = false

-- Janela principal com Key System habilitado
local Window = Rayfield:CreateWindow({
    Name = "Rayfield Mobile Hub",
    Icon = 0,
    LoadingTitle = "Rayfield Hub",
    LoadingSubtitle = "Sistema de Key Ativado",
    ShowText = "OPEN",
    Theme = "Default",

    ToggleUIKeybind = nil, -- Mobile

    ConfigurationSaving = {
        Enabled = true,
        FolderName = "RayfieldMobile",
        FileName = "MobileHub"
    },

    Discord = {
        Enabled = false
    },

    -- KEY SYSTEM (a interface da própria Rayfield pode pedir a key automaticamente)
    KeySystem = true,
    KeySettings = {
        Title = "Sistema de Key",
        Subtitle = "Digite a Key",
        Note = "Key: 1234",
        FileName = "RayfieldKey",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = {"1234"} -- chaves válidas
    }
})

-- Aba principal
local MainTab = Window:CreateTab("Main", 0)
MainTab:CreateSection("Funções")

-- Input manual (fallback) para permitir desbloqueio caso precise
MainTab:CreateInput({
    Name = "Inserir Key (fallback)",
    Placeholder = "Digite a key aqui...",
    RemoveTextAfterFocusLost = false,
    Callback = function(text)
        if text == "1234" then
            unlocked = true
            Rayfield:Notify({
                Title = "Sucesso",
                Content = "Key aceita. Script desbloqueado.",
                Duration = 3
            })
        else
            Rayfield:Notify({
                Title = "Key inválida",
                Content = "A key inserida está incorreta.",
                Duration = 3
            })
        end
    end
})

-- Botão de teste
MainTab:CreateButton({
    Name = "Testar Script",
    Callback = function()
        -- Se a Rayfield tiver seu próprio KeySystem ativo, ela normalmente já validou.
        -- Aqui verificamos o fallback 'unlocked' para garantir comportamento previsível.
        if unlocked or (Rayfield.KeySystem == true) then
            Rayfield:Notify({
                Title = "Sucesso",
                Content = "Key correta! Script liberado.",
                Duration = 3
            })
        else
            Rayfield:Notify({
                Title = "Bloqueado",
                Content = "Por favor, insira a key correta antes de usar esta função.",
                Duration = 3
            })
        end
    end
})

-- Toggle de exemplo
MainTab:CreateToggle({
    Name = "Toggle de Teste",
    CurrentValue = false,
    Callback = function(Value)
        print("Toggle:", Value)
        Rayfield:Notify({
            Title = "Toggle",
            Content = Value and "Ativado" or "Desativado",
            Duration = 2
        })
    end
})# Teste2
