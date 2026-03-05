START

PRINT "Smart Home Security System"

SET systemActive = READ_SYSTEM_STATUS()
SET alarmOn = FALSE

LOOP  // continuous main loop

    // Check if system is active
    IF systemActive = FALSE THEN
        PRINT "System is not active"
        WAIT(5 seconds)
        SET systemActive = READ_SYSTEM_STATUS()
        CONTINUE
    ENDIF

    PRINT "System active: monitoring sensors"

    // Sensor reading
    SET motionDetected = READ_MOTION_SENSOR()
    SET doorOpen = READ_DOOR_SENSOR()
    SET windowOpen = READ_WINDOW_SENSOR()

    // Determine if any threat exists
    IF motionDetected = FALSE AND doorOpen = FALSE AND windowOpen = FALSE THEN
        WAIT(5 seconds)
        CONTINUE
    ENDIF

    // Camera activation condition
    IF motionDetected = TRUE OR doorOpen = TRUE OR windowOpen = TRUE THEN
        ACTIVATE_CAMERA()
    ENDIF

    // False alarm check (homeowner at home?)
    SET ownerAtHome = IS_HOMEOWNER_AT_HOME()

    IF ownerAtHome = TRUE THEN
        PRINT "Possible false alarm: homeowner at home"
        PRINT "Set alarm level to LOW"
        SET alarmLevel = 1
    ELSE
        // Determine alarm level
        IF (doorOpen = TRUE OR windowOpen = TRUE) AND motionDetected = TRUE THEN
            SET alarmLevel = 3   // High
        ELSE IF (doorOpen = TRUE OR windowOpen = TRUE) THEN
            SET alarmLevel = 2   // Medium
        ELSE
            SET alarmLevel = 1   // Low (only motion)
        ENDIF
    ENDIF

    // Trigger alarm and notifications
    SET alarmOn = TRUE
    PRINT "Alarm triggered. Level: " + alarmLevel

    SEND_SMS(alarmLevel)
    SEND_APP_NOTIFICATION(alarmLevel)
    SEND_EMAIL(alarmLevel)

    // Wait and re-check loop
    LOOP
        WAIT(10 seconds)

        PRINT "Re-checking sensors"
        SET motionDetected = READ_MOTION_SENSOR()
        SET doorOpen = READ_DOOR_SENSOR()
        SET windowOpen = READ_WINDOW_SENSOR()

        PRINT "Reset alarm? (YES/NO)"
        READ reset

        IF reset = "YES" THEN
            SET alarmOn = FALSE
            DEACTIVATE_SIREN()
            PRINT "Alarm reset"
            BREAK
        ENDIF

        // Continue if threat still exists
        IF motionDetected = FALSE AND doorOpen = FALSE AND windowOpen = FALSE THEN
            SET alarmOn = FALSE
            DEACTIVATE_SIREN()
            PRINT "No threat detected. Alarm stopped."
            BREAK
        ELSE
            PRINT "Threat continues. Notifications may continue."
            CONTINUE
        ENDIF
    ENDLOOP

    // Update system active status for next cycle
    SET systemActive = READ_SYSTEM_STATUS()

ENDLOOP

END
