# fantas mc game
My new mc game mc 2 // PlayerController.h

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/PlayerController.h"
#include "PlayerController.generated.h"

UCLASS()
class MYGAME_API APlayerController : public APlayerController
{
    GENERATED_BODY()

public:
    // Function to assign CEO status
    void CheckIfFirstPlayerAndAssignRole(const FString& PlayerName);

    // Admin commands that only the CEO can use
    void ExecuteAdminCommand(const FString& Command);

    bool bIsCEO = false;  // To store if the player is CEO or not
};
// PlayerController.cpp

#include "PlayerController.h"
#include "GameFramework/Actor.h"
#include "Engine/World.h"
#include "GameFramework/PlayerState.h"

void APlayerController::CheckIfFirstPlayerAndAssignRole(const FString& PlayerName)
{
    if (PlayerName.Equals("fantaahh"))
    {
        bIsCEO = true;
        UE_LOG(LogTemp, Log, TEXT("%s is now CEO!"), *PlayerName);
    }
    else
    {
        bIsCEO = false;
    }
}

void APlayerController::ExecuteAdminCommand(const FString& Command)
{
    if (!bIsCEO)
    {
        UE_LOG(LogTemp, Warning, TEXT("You must be the CEO to use admin commands."));
        return;
    }

    // Example command: Change time of day
    if (Command.Equals("set_day"))
    {
        GetWorld()->GetTimeSeconds(); // For instance, set to daytime
        UE_LOG(LogTemp, Log, TEXT("Time changed to day."));
    }
    else if (Command.Equals("kick_player"))
    {
        // Code to kick a player
        UE_LOG(LogTemp, Log, TEXT("Player kicked from server."));
    }
    // More commands can be added as needed
}
// AccountSystem.h

#pragma once

#include "CoreMinimal.h"
#include "AccountSystem.generated.h"

USTRUCT(BlueprintType)
struct FPlayerAccount
{
    GENERATED_BODY()

    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    FString Username;

    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    bool bIsCEO;  // CEO status

    // Save this account data (e.g., to a file or SQLite DB)
    void SaveAccount();
};
// AccountSystem.cpp

#include "AccountSystem.h"
#include "Misc/FileHelper.h"

void FPlayerAccount::SaveAccount()
{
    FString AccountData = FString::Printf(TEXT("Username: %s\nCEO: %s"), *Username, bIsCEO ? TEXT("True") : TEXT("False"));
    FFileHelper::SaveStringToFile(AccountData, *FPaths::ProjectSavedDir() + "Accounts/" + Username + ".txt");
}

// Check and assign CEO role when creating account
void CheckAndAssignCEORole(FPlayerAccount& Account)
{
    if (Account.Username.Equals("fantaahh"))
    {
        Account.bIsCEO = true;
    }
    Account.SaveAccount();
}
// YourUIWidget.h (Blueprint UI for Admin Commands)

#pragma once

#include "CoreMinimal.h"
#include "Blueprint/UserWidget.h"
#include "YourUIWidget.generated.h"

/**
 * 
 */
UCLASS()
class MYGAME_API UYourUIWidget : public UUserWidget
{
    GENERATED_BODY()

public:
    // Function to handle Button Click for executing command
    UFUNCTION(BlueprintCallable)
    void OnExecuteAdminCommand();
    
    // Reference to the Command Input Box (TextBox)
    UPROPERTY(BlueprintReadWrite, meta = (BindWidget))
    class UEditableTextBox* CommandTextBox;
};
// YourUIWidget.cpp (Blueprint UI for Admin Commands)

#include "YourUIWidget.h"
#include "GameFramework/PlayerController.h"
#include "Engine/World.h"
#include "PlayerController.h"

void UYourUIWidget::OnExecuteAdminCommand()
{
    FString Command = CommandTextBox->GetText().ToString(); // Assuming CommandTextBox is your text box
    
    APlayerController* PlayerController = GetOwningPlayer();
    if (PlayerController)
    {
        APlayerController* PC = Cast<APlayerController>(PlayerController);
        if (PC)
        {
            PC->ExecuteAdminCommand(Command);
        }
    }
}
// Mobile Controls Setup and Virtual Joystick (for Movement)

1. **Add Virtual Joystick (for Movement):**
   - In Unreal Engine, go to **Content Browser** > **Virtual Joystick**.
   - Drag it into the **HUD** to allow touch-based player movement.

2. **Input Settings:**
   - Go to **Edit > Project Settings > Input**.
   - Set up virtual joystick input and map the movement and buttons to control the player.
