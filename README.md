# codecode

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" type="text/css" href="style.css">
    <title>Submit Your Details</title>
    <style>
        html {
            font-size: 16px;
        }

        body {
            background-color: rgba(98, 95, 241, 0.635);
            margin: 0;
            font-family: 'Times New Roman', Times, serif; /* Changed font to Times New Roman */
            color: white;
        }

        .container {
            width: 100%;
            max-width: 720px; /* Set a max-width for larger screens */
            margin: 2rem auto; /* Center the container with fixed margins */
            background: rgba(21, 20, 85, 0.8);
            padding: 2rem;
            border-radius: 0.5rem;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5); /* Optional shadow */
        }

        header {
            text-align: center;
            margin-bottom: 1.5rem;
        }

        h1 {
            font-weight: 400;
            color: white;
        }

        p {
            color: rgba(255, 255, 255, 0.8);
        }

        #userForm {
            background: rgba(255, 255, 255, 0.9);
            padding: 1.5rem;
            border-radius: 0.3rem;
        }

        .form-input {
            margin: 0 0 1.2rem 0; /* Adjusted for uniformity */
        }

        .form-input-size {
            display: block;
            width: 100%;
            height: 2.5rem; /* Increased height for better touch targets */
            padding: 0.5rem; /* Increased padding */
            border-radius: 0.2rem;
            outline: 0;
            border: 1px solid #ccc; /* Add border for inputs */
            margin-top: 0.4rem;
        }

        .error {
            color: red;
            font-size: 0.9em;
            margin-top: 0.2rem;
        }

        #submit {
            width: 100%;
            padding: 0.8rem;
            background: rgb(7, 173, 7);
            color: white;
            border-radius: 0.2rem;
            cursor: pointer;
            border: none;
            margin-top: 1rem;
        }

        #submit:disabled {
            background-color: grey;
            cursor: not-allowed;
        }

        .terms-container {
            display: flex;
            align-items: center;
            margin-top: 10px;
        }

        .terms-label {
            font-size: 0.9em;
            margin-left: 2px;
        }

        /* Responsive Styles */
        @media (max-width: 414px) {
            .container {
                margin: 1rem; /* Smaller margin for smaller screens */
                padding: 1rem; /* Adjust padding for smaller screens */
            }

            .form-input-size {
                height: 2rem; /* Decrease height on smaller screens */
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Submit Your Details</h1>
            <p>Thank you for taking the time to fill out this form</p>
        </header>

        <form id="userForm">
            <div class='form-input'>
                <label for="firstname">First Name</label>
                <input type="text" id="firstname" name="firstname" placeholder="First Name" class="form-input-size" required autocomplete="given-name">
                <div class="error" id="firstnameError"></div>
            </div>

            <div class='form-input'>
                <label for="lastname">Last Name</label>
                <input type="text" id="lastname" name="lastname" placeholder="Last Name" class="form-input-size" required autocomplete="family-name">
                <div class="error" id="lastnameError"></div>
            </div>

            <div class='form-input'>
                <label for="email">Email Address</label>
                <input type="email" id="email" name="email" placeholder="Email Address" class="form-input-size" required autocomplete="email">
                <div class="error" id="emailError"></div>
            </div>

            <div class='form-input'>
                <label for="countrycode">Country Code</label>
                <select id="countrycode" name="countrycode" class="form-input-size" required>
                    <option value="">Select Country Code</option>
                    <option value="+1">USA (+1)</option>
                    <option value="+91">India (+91)</option>
                    <option value="+44">UK (+44)</option>
                </select>
                <div class="error" id="countrycodeError"></div>
            </div>

            <div class='form-input'>
                <label for="mobileno">Mobile No.</label>
                <input type="text" id="mobileno" name="mobileno" placeholder="Mobile No." class="form-input-size" required autocomplete="tel">
                <div class="error" id="mobilenoError"></div>
            </div>

            <div class='form-input terms-container'>
                <input type="checkbox" id="terms" required>
                <label for="terms" class="terms-label">I agree to the Terms & Conditions</label>
            </div>

            <div class='form-input'>
                <button type="submit" class="submit" disabled id="submit">Submit Record</button>
                <button type="button" class="updateDelete">Update/Delete</button>
            </div>
        </form>

        <div id="responseMessage"></div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            function validateForm() {
                const firstname = $('#firstname').val();
                const lastname = $('#lastname').val();
                const email = $('#email').val();
                const countrycode = $('#countrycode').val();
                const mobileno = $('#mobileno').val();
                const isTermsChecked = $('#terms').is(':checked');
                const emailValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
                const mobileValid = /^\d{10}$/.test(mobileno);
                
                // Reset error messages
                $(".error").text("");

                let isValid = true;

                if (!firstname) {
                    $("#firstnameError").text("* This field is required.");
                    isValid = false;
                }

                if (!lastname) {
                    $("#lastnameError").text("* This field is required.");
                    isValid = false;
                }

                if (!emailValid) {
                    $("#emailError").text("* Please enter a valid email address.");
                    isValid = false;
                }

                if (!countrycode) {
                    $("#countrycodeError").text("* Please select a country code.");
                    isValid = false;
                }

                if (!mobileValid) {
                    $("#mobilenoError").text("* Mobile number must contain exactly 10 digits.");
                    isValid = false;
                }

                if (!isTermsChecked) {
                    $("#mobilenoError").append("<br>* You must agree to the terms.");
                    isValid = false;
                }

                $("#submit").prop("disabled", !isValid);
            }

            // Enable/disable submit button based on form validity
            $("input, select").on("input change", validateForm);
            $("#terms").on("change", validateForm);

            $("#userForm").on("submit", function(event) {
                event.preventDefault();

                const firstname = $('#firstname').val();
                const lastname = $('#lastname').val();
                const email = $('#email').val();
                const countrycode = $('#countrycode').val();
                const mobileno = $('#mobileno').val();

                $.ajax({
                    type: "POST",
                    url: "https://mcsz6740-shssv34d-jxn10f-nm8.pub.sfmc-content.com/jn4ta43fo1r",
                    crossDomain: true,
                    dataType: 'text',
                    data: {
                        firstname: firstname,
                        lastname: lastname,
                        email: email,
                        countrycode: countrycode,
                        mobileno: mobileno
                    },
                    success: function(data) {
                        $("#responseMessage").html("<p>Record inserted successfully!</p>");
                    },
                    error: function(jqXHR, textStatus, errorThrown) {
                        $("#responseMessage").html("<p>Error: " + jqXHR.responseText + "</p>");
                    }
                });
            });

            $(".updateDelete").on("click", function () {
                window.location.href = "file:///C:/Users/Nikhil.Thakur04/Documents/Code/form2.html";
            });
        });
    </script>
</body>
</html>
