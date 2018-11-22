# Better way to validate client REQUEST

While making a REST API it is always mandatory to validate what you are getting from the client and this thing is so common. I spoke to a lot of people and I have seend production code that uses `if-else` ladder to validate requests.
Something like this:

```javascript
if (!req.body.todoName) {
  res
    .status(400)
    .json({ status: false, message: 'TodoName name is mandatory.' });
  return;
}
if (!req.body.todoDescription) {
  res
    .status(400)
    .json({ status: false, message: 'Todo description is mandatory.' });
  return;
}
```

## Using Request Vaidators

Let's have a look at a better way to validate. Here is have used `express-validator` and you can have a look at `Joi` as well.

```javascript
// inside controllers/random.controller.js
const deleteSlot = Router();

function validateRequest(req) {
  req
    .checkBody('vmId', 'vmId should be integer')
    .isInt()
    .exists();

  req
    .checkBody('mvId', 'mvId should be integer')
    .isInt()
    .exists();

  req
    .checkBody('slotIdentifier', 'slotIdentifier should be integer')
    .isInt()
    .exists();

  return req.validationErrors();
}

deleteSlot.route('/slots').delete(async (req, res) => {
  try {
    const errors = validateRequest(req);
    if (errors) {
      return sendResponse(res, 422, {}, errors[0].msg);
    }
    const { vmId, mvId, slotIdentifier } = req.body;
    // call delete service here
    return sendResponse(res, 200, {}, 'Successfully deleted slot');
  } catch (err) {
    return sendResponse(res, 500, {}, err.message);
  }
});
```
