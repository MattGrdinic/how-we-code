# Deliver Solutions that Work as Expected

|                    |                 |
| ------------------ | --------------- |
| **Content Owners** | @Owner |
| **Last Updated**   | 2025-10-02      |

---

## Other References

- [Unit Testing Guidelines: What to Test And What Not](https://www.automatetheplanet.com/unit-testing-guidelines/)

## Overview

---

Quality starts way before we write any code. We pick simple architectures and tools that help us keep our code clean and easy to maintain. Having a strong foundation makes everything better. Since no single method can guarantee quality, we test everything, even the docs.

- Unit tests check each function on its own.
- End-to-end tests mimic how users work with the software.
- Performance and load tests make sure the software can grow smoothly.
- Data validation helps catch issues early on.
- Process documents stay clear and up-to-date

## Definition of Done

---

- Your `README.md` helps engineers get what the project is about and stays up to date.
- You've updated the `CHANGELOG.md` with the latest changes.
- Logs and traces are being captured in Datadog, indexes are set up for log ingestion, and the correct retention policies are in place.
- You've set up monitoring and alerts to keep an eye on things.
- You've written down how the system runs and made sure the instructions are easy to follow.
- Scripts used to run the solution have been tested and work well.
- You've set up regular tasks to keep operations smooth.
- Your CI build runs all the unit and end-to-end tests.
- Pull requests are finished, all comments are handled, and approved—you can't approve your own PR.
- You've run security and vulnerability scans and checked the results.

## Unit Tests

---

Our code should be easy to test with unit tests, and these tests shouldn't depend on outside resources. We want to cover as much code as makes sense. For example, if a file has 20% coverage but the rest is just constants, that's okay since testing constants doesn't really help. Lots of languages and tools let you skip this kind of code when measuring coverage, so use those options when it fits.

### Things Not to Test

- 3rd party API or code packages
- Constants
- Auto-generated code
- Types that only have attributes and no behavior.

This list shows common examples but isn't complete. Skip unit tests if the team thinks they're not worth the time. Make sure the reasons for skipping are clear and easy to understand for anyone reading later.

## End-to-end Testing

---

End-to-end tests make sure your system works just like your users expect. If you built a RESTful API, you'll want E2E tests that check _every_ possible response from your endpoints. You can find examples of using BATS for this kind of testing here: <https://company.visualstudio.com/Team/_git/project?path=/tests> and here: <https://company.visualstudio.com/Team/_git/project?path=/tests>

These tests run in separate environments using Docker to start everything needed—like infrastructure, services, databases, and more. Usually, they do the following:

1. Send a command to your API using `cURL` (like `app-api`) or run the tool you're testing (like `midctl`). BATS was used to create the tests for MID MGMT API, but you can use other languages. Your E2E tests must not depend on the code implementing your service.
2. Verify the response matches your expectations. For APIs, test all endpoints and their possible responses.
3. If your test changes data, check the database to confirm the response matches what was stored. For example, if a test creates, updates, or deletes a row, verify the change in the database.

Just like unit tests, your E2E tests should run automatically in your CI build.

## Testing Your Instructional Docs

---

> **The Ideal**
>
> The ideal goal is that a person who just onboarded to our team a few days ago can read through the document and succeed!

People often miss this, but it's really important. We need to make sure our documents are aimed at the right people, easy to get, and correct. Testing your docs is just as important as testing your code. Why? Because someone else will probably have to use or manage the software, not just you. You're not writing for yourself; you're writing for your team. Help them succeed!

It's a manual process, so here are some steps you'll need to take.

1. Keep it short and simple. Skip any extra words.
2. Use AI tools to make your writing clearer and consistent.
3. Have a teammate try following your instructions. If they ask questions, it means your document needs more details or clearer explanations.
4. Once someone can follow the instructions without asking anything, your document is good to go.

## Revision History

---

- 2025-10-02: @Owner - Complete rewrite with current standards
