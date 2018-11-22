# Format for sending API response

In almost all the API repository I see, the following code block is very common there:

```javascript
res.status(200).send(data);
res.status(404).send({ message: 'Not found' });
res.status(500).send({ error: 'Internal server error happened' });
```

So what is the problem with the above code or the way of sending response? I found the following:

- `.status()` is repeated
- `send()` do not have a standard format for sending response which makes it really hard for domain developers to parse the response.

## My Solution

I made a utility function to send response like this:

```javascript
/**
 * @function
 *
 * This function is a response sending wrapper
 * Instead of writing extra repetitive lines
 * use this wrapper
 *
 * Made with DRY in mind
 *
 * @author Ashok Dey <https://github.com/ashokdey>
 *
 * @param {object} res the response object
 * @param {number} statusCode the http status code
 * @param {array | object } data the data you want to send with the response
 * @param {string} message the message you want to send for success/failure
 */

export default function sendResponse(res, statusCode, data = {}, message) {
  if (typeof statusCode !== 'number') {
    throw new Error('statusCode should be a number');
  }

  // status variable to store the status of the response either success or failed
  let status = null;

  // regex pattern to validate that the status code is always 3 digits in length
  const lengthPattern = /^[0-9]{3}$/;

  // check for the length of the status code, if its 3 then set default value for status as
  // failed
  // else throw an error
  if (!lengthPattern.test(statusCode)) {
    throw new Error('Invalid Status Code');
  }

  // regex to test that status code start with 2 or 3 and should me 3 digits in length
  const pattern = /^(2|3)\d{2}$/;

  // if the status code starts with 2, set satus variable as success
  pattern.test(statusCode) ? (status = 'success') : (status = 'failed');

  return res.status(statusCode).json({
    status,
    data,
    message
  });
}
```

### Usage

```javascript
// inside src/controllers/users.controllers.js
import log4js from 'log4js';
const logger = log4js.getLogger('UserModule');

import { isActionAllowed } from '../../utils/isAllowedToAccess';
import { sendResponse, handleCustomThrow } from '../../utils';
import { getAllUsersService } from '../../services/users/users.services';

export async function getUsers(req, res) {
  try {
    // check the user got permission to access user list
    const checkAction = await isActionAllowed(req.user.id, 'users');
    if (!checkAction || !checkAction.read) {
      return sendResponse(res, 403, {}, 'Not allowed');
    }

    const data = await getAllUsersService();
    return sendResponse(res, 200, { users: data }, 'Fetched data successfully');
  } catch (error) {
    logger.error('error in get all users: ', error);
    return handleCustomThrow(res, error);
  }
}
```

I have good memories with the above code block and it's now 2 years old. This highly impressed [Bikash Dash](https://github.com/beeeku).

Credits:

- Erlier data was sent as an empty array whcih created a barrier to add extra keys in the data. So data should be returned as an object and this was coined by [Sahil Chitkara](https://github.com/sahilchitkara)

### Bonus :blush:

Not exactly a bonus, in case you want to see whats inside `handleCustomThrow`

```javascript
// inside /src/utils/handleCustomThrow.js
import sendResponse from './sendResponse';
export { sendResponse, customValidators, jwtUtils };

export function handleCustomThrow(res, error) {
  if (error.code === 400) {
    return sendResponse(res, error.code, {}, error.msg);
  }
  if (error.code === 401) {
    return sendResponse(res, error.code, {}, error.msg);
  }
  if (error.code === 403) {
    return sendResponse(res, error.code, {}, error.msg);
  }
  if (error.code === 404) {
    return sendResponse(res, error.code, {}, error.msg);
  }
  return sendResponse(res, 500, {}, 'Something went wrong');
}
```
