var url = window.location !== window.parent.location ? document.referrer : document.location.href;
if (!isBlackListed(url)) {
    if (location.href.includes("google.com/recaptcha")) {
        var clickCheck = setInterval(function() {
            if (document.querySelectorAll(".recaptcha-checkbox-checkmark").length > 0) {
                clearInterval(clickCheck);
                document.querySelector(".recaptcha-checkbox-checkmark").click();
            }
        }, 100);
    } else {
        window.onload = readyToHelp;
    }
}

function readyToHelp() {
    try {
        grecaptcha.execute();
    } catch(e) {}
    [...document.forms].forEach(form => {
        if (form.innerHTML.includes("google.com/recaptcha")) {
            var solveCheck = setInterval(function() {
                if (grecaptcha.getResponse().length > 0) {
                    clearInterval(solveCheck);
                    form.submit();
                }
            }, 100);
        }
    });
}

function isBlackListed(url) {
    return blacklistedUrls.filter(bu => url.includes(bu)).length > 0;
}
