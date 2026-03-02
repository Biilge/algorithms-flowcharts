PRINT "Please insert your card."
READ cardInserted

SET pinAttempts = 0
SET pinVerified = FALSE

LOOP (pinVerified = FALSE AND pinAttempts < 3)

    PRINT "Enter your PIN:"
    READ enteredPIN

    IF enteredPIN = storedPIN THEN
        SET pinVerified = TRUE
    ELSE
        SET pinAttempts = pinAttempts + 1
        PRINT "Incorrect PIN."

        IF pinAttempts < 3 THEN
            PRINT "Try again."
        ENDIF
    ENDIF

ENDLOOP

IF pinVerified = FALSE THEN
    PRINT "Card blocked due to 3 incorrect PIN entries."
    PRINT "Please contact your bank."
    BREAK (exit MainTransactionLoop)
ENDIF

PRINT "PIN verified."

// Query account information
SET balance = QUERY_ACCOUNT_BALANCE()
SET todayWithdrawn = QUERY_TODAY_WITHDRAWN_TOTAL()
SET dailyLimit = QUERY_DAILY_WITHDRAWAL_LIMIT()

PRINT "Your current balance is: " + balance + " TL"

// Withdrawal process loop (keeps asking amount until valid or user cancels)
SET withdrawalCompleted = FALSE

LOOP (withdrawalCompleted = FALSE)

    PRINT "Enter withdrawal amount in TL (or enter 0 to cancel):"
    READ amount

    IF amount = 0 THEN
        PRINT "Withdrawal cancelled."
        SET withdrawalCompleted = TRUE
    ELSE

        // Check: Multiple of 20 TL
        IF amount MOD 20 != 0 THEN
            PRINT "Invalid amount. Amount must be a multiple of 20 TL."
        ELSE

            // Check: Sufficient balance
            IF amount > balance THEN
                PRINT "Insufficient balance."
            ELSE

                // Check: Daily withdrawal limit
                IF (todayWithdrawn + amount) > dailyLimit THEN
                    PRINT "Daily withdrawal limit exceeded."
                    PRINT "Remaining daily limit is: " + (dailyLimit - todayWithdrawn) + " TL"
                ELSE
                    // All checks passed -> dispense cash
                    DISPENSE_CASH(amount)
                    PRINT_RECEIPT(amount, balance - amount)

                    SET balance = balance - amount
                    SET todayWithdrawn = todayWithdrawn + amount

                    PRINT "Please take your cash."
                    PRINT "Transaction successful."
                    SET withdrawalCompleted = TRUE
                ENDIF

            ENDIF

        ENDIF

    ENDIF

ENDLOOP

PRINT "Would you like to perform another transaction? (YES/NO)"
READ again

IF again != "YES" THEN
    PRINT "Thank you for using the ATM. Goodbye."
    BREAK (exit MainTransactionLoop)
ENDIF
