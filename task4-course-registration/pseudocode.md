START

PRINT "University Course Registration System"

PRINT "Enter Student ID"
READ studentID

PRINT "Enter Password"
READ password

IF LOGIN_VALID(studentID, password) = FALSE THEN
    PRINT "Login failed"
    END
ENDIF

PRINT "Login successful"

SET selectedCourses = empty
SET totalCredits = 0

LOOP

    PRINT "Choose action:"
    PRINT "1 View Course List"
    PRINT "2 Add Course"
    PRINT "3 Drop Course"
    PRINT "4 Finish Registration"
    READ action

    IF action = 4 THEN
        BREAK
    ENDIF

    IF action = 1 THEN
        DISPLAY courseList
    ENDIF

    IF action = 2 THEN

        PRINT "Enter course code to add"
        READ course

        IF COURSE_CAPACITY_FULL(course) THEN
            PRINT "Course quota is full"
            CONTINUE
        ENDIF

        IF PREREQUISITE_NOT_COMPLETED(studentID, course) THEN
            PRINT "Prerequisite not satisfied"
            CONTINUE
        ENDIF

        IF SCHEDULE_CONFLICT(selectedCourses, course) THEN
            PRINT "Schedule conflict detected"
            CONTINUE
        ENDIF

        SET courseCredits = GET_CREDITS(course)

        IF totalCredits + courseCredits > 35 THEN
            PRINT "Credit limit exceeded"
            CONTINUE
        ENDIF

        ADD course to selectedCourses
        SET totalCredits = totalCredits + courseCredits

        PRINT "Course added successfully"

    ENDIF

    IF action = 3 THEN

        PRINT "Enter course code to drop"
        READ dropCourse

        IF dropCourse in selectedCourses THEN
            REMOVE dropCourse from selectedCourses

            SET courseCredits = GET_CREDITS(dropCourse)
            SET totalCredits = totalCredits - courseCredits

            PRINT "Course removed"
        ELSE
            PRINT "Course not found in your list"
        ENDIF

    ENDIF

ENDLOOP

PRINT "Checking advisor approval requirement"

SET gpa = GET_GPA(studentID)

IF gpa < 2.5 THEN
    PRINT "Advisor approval required"
    SEND_APPROVAL_REQUEST(studentID)
ENDIF

PRINT "Registration Summary"
DISPLAY selectedCourses
PRINT "Total Credits:", totalCredits

PRINT "Confirm registration? (YES/NO)"
READ confirm

IF confirm = "YES" THEN
    SAVE_REGISTRATION(studentID, selectedCourses)
    PRINT "Registration completed successfully"
ELSE
    PRINT "Registration cancelled"
ENDIF

END
