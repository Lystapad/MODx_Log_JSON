# MODx_Log_JSON
MODx Login module Returns an error message in JSON format in the response and returns a JSON error by default if the requested login or password value is incorrect. intercepted by the corresponding script
Modified by Lystapad BY https://github.com/Lystapad Returns an error message in JSON format in the response and returns a JSON error by default if the requested login or password value is incorrect. intercepted by the corresponding script

Disable selection conditions in lines 311 and 325 to return a message in JSON format without acceptance:
yoursite\core\components\login\controllers\web\Login.php

    311  // if ($this->getProperty('jsonResponse')) {
    ---
    319  // }

    325 // if ($this->getProperty('jsonResponse')) {
    ---
    332 // }

Accordingly, in the JS code we add the corresponding interception, something like this

			if (contentType && contentType.includes("application/json")) {
				response.json().then(data => {
					console.log("JSON", data);
					let success = data.success, message = data.message, errors = data.errors;
					if (!success) {
						if (errors) spans.forEach((element) => {
							element.textContent = data.errors[element.dataset.errors] || "";
						});
						event.target.querySelector(".inputError").textContent = data.message;
					} else if (success && message) {
						console.log('event.target', event.target);
						event.target.reset();
						event.target.outerHTML = data.message;
						// event.target.textContent = data.message;
					} else {
						event.target.reset();
						location.reload();
					}
				});
			}

