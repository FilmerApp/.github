# Business Process - Creating New Interests

A business process is a collection of tasks, often organised in a flowchart, that, when performed by the right employees or equipment, will result in a service being performed for a customer. For example, a business process may highlight how exactly a customer can create a new bank account, or how a customer might be sent a replacement package for an item that arrived broken.

Unfortunately due to the nature of Eeventify, being more like a social media application, there are very few involved processes required for its day-to-day operation. We don’t deliver packages, take customer orders, or anything else that would regularly require tasks to be performed by an employee.

There is one small exception, to do with the way we manage interests. When an event is created, a user can add related interest, for example adding ‘sports’, ‘outdoors’, and ‘soccer’ to a pick-up soccer game they want to organize. To protect from misuse, and because all of these interests could potentially be added to someone’s profile in order to filter their event feed, users are only be able to pick the interests from a set list. Of course, it’s possible that one of the interests that best describes the event isn’t on the list at all, in which case the user will have to request for a specific interest to be added.

The process involved is outlined in the model below.

![Eeventify Business Process Model](https://github.com/FilmerApp/.github/blob/main/images/Eeventify%20Business%20Process.png)

Whenever a user requests for a new interest to be added, it will have to be manually vetted for suitability by an employee first, who will make sure the interest is appropriate, understandable, not too niche, etc. This employee is called the ‘content screener’ in the model.

Crucially, the user’s event is created immediately, instead of having to wait for the interest to be approved, considering this process will most likely not be instantaneous. If the interest does get approved, the content screener can manually add it to the event after it has already been created. The new interest will also be added to the general interest database, so users can immediately add it to new events in the future.

The middle layer (where the event is created), will be handled by the Eeventify system itself (specifically our Event Service), not requiring any approval or action from employees.

Ideally, our application will require as little oversight or employee interference as possible, but this situation highlights one specific scenario where performing some tasks might be required.
