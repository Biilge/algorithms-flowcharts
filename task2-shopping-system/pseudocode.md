START

PRINT "Welcome to the Online Shopping System."

PRINT "Are you logged in? (YES/NO)"
READ isLoggedIn

IF isLoggedIn != "YES" THEN
    PRINT "Enter username:"
    READ username

    PRINT "Enter password:"
    READ password

    IF LOGIN_VALID(username, password) = FALSE THEN
        PRINT "Login failed. Exiting system."
        END
    ENDIF

    PRINT "Login successful."
ENDIF

SET cartItems = empty
SET discountAmount = 0

LOOP

    PRINT "Choose action: BROWSE / CART / CHECKOUT / EXIT"
    READ action

    IF action = "EXIT" THEN
        PRINT "Goodbye."
        BREAK
    ENDIF

    IF action = "BROWSE" THEN

        LOOP

            PRINT "Enter category name or type BACK"
            READ category

            IF category = "BACK" THEN
                BREAK
            ENDIF

            PRINT "Show products in category"
            PRINT "Enter product ID or BACK"
            READ productID

            IF productID = "BACK" THEN
                CONTINUE
            ENDIF

            PRINT "Enter quantity"
            READ qty

            IF qty <= 0 THEN
                PRINT "Invalid quantity"
                CONTINUE
            ENDIF

            SET stock = QUERY_STOCK(productID)

            IF qty > stock THEN
                PRINT "Insufficient stock"
                CONTINUE
            ENDIF

            ADD productID and qty to cartItems
            PRINT "Product added to cart"

        ENDLOOP

    ENDIF

    IF action = "CART" THEN

        LOOP

            PRINT "View cart"
            PRINT "Choose: EDIT / REMOVE / CLEAR / BACK"
            READ cartAction

            IF cartAction = "BACK" THEN
                BREAK
            ENDIF

            IF cartAction = "CLEAR" THEN
                CLEAR cartItems
                PRINT "Cart cleared"
                CONTINUE
            ENDIF

            IF cartAction = "EDIT" THEN
                PRINT "Enter product ID"
                READ editID

                PRINT "Enter new quantity"
                READ newQty

                IF newQty <= 0 THEN
                    PRINT "Invalid quantity"
                    CONTINUE
                ENDIF

                SET stock = QUERY_STOCK(editID)

                IF newQty > stock THEN
                    PRINT "Insufficient stock"
                    CONTINUE
                ENDIF

                UPDATE quantity in cartItems
                PRINT "Cart updated"
                CONTINUE
            ENDIF

            IF cartAction = "REMOVE" THEN
                PRINT "Enter product ID"
                READ removeID

                REMOVE item from cartItems
                PRINT "Item removed"
                CONTINUE
            ENDIF

        ENDLOOP

    ENDIF

    IF action = "CHECKOUT" THEN

        IF cartItems is empty THEN
            PRINT "Cart is empty"
            CONTINUE
        ENDIF

        SET subtotal = CALCULATE_SUBTOTAL(cartItems)
        PRINT "Subtotal:", subtotal

        PRINT "Apply discount code? (YES/NO)"
        READ discountChoice

        IF discountChoice = "YES" THEN
            PRINT "Enter discount code"
            READ code

            IF VALIDATE_DISCOUNT_CODE(code) = TRUE THEN
                SET discountAmount = CALCULATE_DISCOUNT(subtotal)
                PRINT "Discount applied"
            ELSE
                PRINT "Invalid discount code"
                SET discountAmount = 0
            ENDIF
        ENDIF

        SET total = subtotal - discountAmount

        IF total < 10 THEN
            PRINT "Minimum order amount is 10"
            CONTINUE
        ENDIF

        IF total >= 40 THEN
            SET shipping = 0
            PRINT "Free shipping"
        ELSE
            SET shipping = 5
            PRINT "Shipping fee applied"
        ENDIF

        SET finalTotal = total + shipping
        PRINT "Final total:", finalTotal

        PRINT "Select payment method: CARD / CASH_ON_DELIVERY"
        READ paymentMethod

        IF paymentMethod = "CARD" THEN
            PRINT "Enter card details"
            READ cardInfo

            IF PROCESS_PAYMENT(finalTotal) = FALSE THEN
                PRINT "Payment failed"
                CONTINUE
            ENDIF

            PRINT "Payment successful"

        ELSE IF paymentMethod = "CASH_ON_DELIVERY" THEN
            PRINT "Cash on delivery selected"

        ELSE
            PRINT "Invalid payment method"
            CONTINUE
        ENDIF

        PRINT "Confirm order? (YES/NO)"
        READ confirm

        IF confirm = "YES" THEN
            CREATE_ORDER
            CLEAR cartItems
            PRINT "Order confirmed"
        ELSE
            PRINT "Order cancelled"
        ENDIF

    ENDIF

ENDLOOP

END
