# validation
<script src="http://cdnjs.cloudflare.com/ajax/libs/jquery-form-validator/2.3.26/jquery.form-validator.min.js"></script>
(function ($, window) {

    if ($("html").hasClass("ie8")) {
        /* do your things */
    };

    // Add a new validator
    $.formUtils.addValidator({
        name: 'ukpostcode',
        validatorFunction: function (val, $el, config, language, $form) {
            return isValidPostcode(val);
        },
        errorMessage: 'Please enter valid postcode',
        errorMessageKey: 'badPostcode'
    });

    $.formUtils.addValidator({
        name: 'ukphone',
        validatorFunction: function (val, $el, config, language, $form) {
            return isValidUKPhone(val);
        },
        errorMessage: 'Please enter valid phone number',
        errorMessageKey: 'badPhoneNumber'
    });

    function isValidPostcode(val) {
        var regexp = /^[a-zA-Z]{1,2}[0-9][0-9A-Za-z]? {0,1}[0-9][a-zA-Z]{2}$/;
        return regexp.test(val);
    }

    function isValidUKPhone(val) {
        var regexp = /^(((\+44\s?\d{4}|\(?0\d{4}\)?)\s?\d{3}\s?\d{3})|((\+44\s?\d{3}|\(?0\d{3}\)?)\s?\d{3}\s?\d{4})|((\+44\s?\d{2}|\(?0\d{2}\)?)\s?\d{4}\s?\d{4}))(\s?\#(\d{4}|\d{3}))?$/;
        return regexp.test(val);
    }

    window.applyValidation = function (validateOnBlur, forms, messagePosition, scrollToTopOnError, ajaxMode, callback) {
        if (!forms)
            forms = 'form';

        if (!messagePosition)
            messagePosition = 'top';

        if (scrollToTopOnError == undefined) {
            scrollToTopOnError = true;
        }

        if (ajaxMode == undefined)
            ajaxMode = false;

        $.validate({
            form: forms,
            language: {
                requiredFields: '*',
                errorTitle: 'Please correct the errors shown below !'
            },
            validateOnBlur: validateOnBlur,
            errorMessagePosition: messagePosition,
            scrollToTopOnError: scrollToTopOnError,
            lang: 'en',
            borderColorOnError: 'red',
            onModulesLoaded: function () {
            },
            onValidate: function ($f) {
            },
            onError: function ($form) {
            },
            onSuccess: function ($form) {
                if (callback)
                    callback($form[0].id);
                return !ajaxMode;
            },
            onElementValidate: function (valid, $el, $form, errorMess) {
                var isFormValid = $('#' + $form[0].id).isValid({}, {}, false);
                if (isFormValid) {
                    $('.alert-danger').hide();
                }
                else {
                    $('.alert-danger').show();
                }
            }
        });
    };

    window.applyValidation(true, '#findBrokerForm', 'top', true, false);
    window.applyValidation(true, '#contactForm', $('#error-container'), false, true, submitContact);
    window.applyValidation(true, '#mainForm', 'top', true, false);
})(jQuery, window);
