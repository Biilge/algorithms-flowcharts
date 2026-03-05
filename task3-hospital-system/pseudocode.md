START

PRINT "Welcome to Hospital System"

PRINT "Enter your ID number"
READ idNumber

IF VERIFY_ID(idNumber) = FALSE THEN
    PRINT "Identity verification failed"
    END
ENDIF

PRINT "Identity verified"

LOOP

    PRINT "Select action:"
    PRINT "1 - Make Appointment"
    PRINT "2 - View Test Results"
    PRINT "3 - Exit"
    READ action

    IF action = 3 THEN
        PRINT "Goodbye"
        BREAK
    ENDIF

    IF action = 1 THEN

        PRINT "Appointment Module"

        PRINT "Select clinic"
        READ clinic

        PRINT "Display doctor list for selected clinic"
        SET doctorList = GET_DOCTORS(clinic)

        LOOP
            PRINT "Select doctor (or type BACK to return)"
            READ doctor

            IF doctor = "BACK" THEN
                BREAK
            ENDIF

            PRINT "Display available time slots for selected doctor"
            SET timeSlots = GET_AVAILABLE_SLOTS(doctor)

            IF timeSlots is empty THEN
                PRINT "No available slots, choose another doctor"
                CONTINUE
            ENDIF

            LOOP
                PRINT "Select a time slot (or type BACK to choose another doctor)"
                READ slot

                IF slot = "BACK" THEN
                    BREAK
                ENDIF

                IF SLOT_IS_AVAILABLE(doctor, slot) = FALSE THEN
                    PRINT "Slot not available, choose another slot"
                    CONTINUE
                ENDIF

                PRINT "Confirm appointment? (YES/NO)"
                READ confirm

                IF confirm = "YES" THEN
                    CREATE_APPOINTMENT(idNumber, clinic, doctor, slot)
                    PRINT "Appointment confirmed"
                    SEND_SMS(idNumber, "Your appointment is confirmed")
                    PRINT "SMS notification sent"
                    BREAK
                ELSE
                    PRINT "Appointment not confirmed"
                    BREAK
                ENDIF

            ENDLOOP

            BREAK

        ENDLOOP

    ENDIF

    IF action = 2 THEN

        PRINT "Test Results Module"

        IF TESTS_EXIST(idNumber) = FALSE THEN
            PRINT "No tests found"
        ELSE
            IF RESULTS_READY(idNumber) = FALSE THEN
                PRINT "Results are not ready yet, please wait"
            ELSE
                SET results = GET_RESULTS(idNumber)
                PRINT "Displaying test results"
                DISPLAY results

                PRINT "Download results as PDF? (YES/NO)"
                READ download

                IF download = "YES" THEN
                    GENERATE_PDF(results)
                    PRINT "PDF downloaded"
                ENDIF
            ENDIF
        ENDIF

    ENDIF

    IF action != 1 AND action != 2 AND action != 3 THEN
        PRINT "Invalid selection"
    ENDIF

    PRINT "Would you like to perform another action? (YES/NO)"
    READ again

    IF again != "YES" THEN
        PRINT "Goodbye"
        BREAK
    ENDIF

ENDLOOP

END
