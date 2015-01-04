---
layout: post
title: "Keeping client-side apps light"
date: 2015-01-04 15:17:25 +0530
comments: true
categories: 
---
Let's assume for an iOS app you're building, you have a requirement to display user's name. You check with the API team, they suggest using the user profile API. The name field in the user profile API response looks something like this:

      "name": {
        "first": "Mark",
        "last": "Hall"
       }

Your product owner would like to see the full name, so you write code in the app to concatenate the first name and the last name. Everything looks good, and your changes go live.

After a couple of days, the same requirement is raised for the Android app. Android app developer checks how the full name was derived in the iOS app, goes ahead with a similar implementation.

At this point, logic to generate full name is duplicated in both the apps. In this particular example, duplication probably seems fine since the logic is not complicated. But if you keep adding small bits of logic like this in the client-side apps, you'll end up with a lot of duplication. And this duplication will increase significantly as you write new apps to support multiple platforms.

To avoid this sort of duplication, we need to keep logic on the server-side as much as possible. There are multiple benefits of moving logic to the servers:

* First and the obvious one is to reduce duplication of logic on the client side. Client-side apps will simply see the required information coming back from the APIs. In the example above, we'll get a full name field along with the first and last name. The app is free to use whichever field makes most sense for the use case.
* Secondly, any defect related to logic shifted to the server-side can be fixed without requiring an app update.
* Lastly, changes can be applied without the need to release an update to the app. For example, if the back-end starts capturing middle name along with the first and last name fields, you'll simply update the logic to generate full name.

This bring us to the question, how can logic be shifted to the servers?

Since server-side API development is a specialized skill, it's un-fair to expect client-side app developers to design great APIs. Thus, most organizations(specially large ones) have separate teams to manage their back-end services. But this does not mean client-side developers can't leverage server-side skills available in-house.

I'm listing three approaches that can be used to shift logic to servers:

#### Poly-skilled teams

When forming a team, ensure the team has all development skills required to manage the end-to-end stack. For example, a single team can have developers doing iOS, Android and server-side API development.

Such a team should be empowered to take on changes in any repository that impacts their work. So in the example above, the server-side API developer would simply clone the user profile API code and make necessary changes to send back a full name field for the apps to consume.

This approach is the most efficient but requires a level of maturity difficult to find in most organizations. Startups are an example where such an approach is easy to implement as everyone virtually knows everything there is to know.

#### Get the API teams to make necessary changes

When a requirement comes in, analyze the available API response and have a conversation with the API team to make changes in the API response itself. In the example above, this would mean asking for a full name field in the response.

As long as the API response is not changing existing fields, and you are simply expanding the contract by adding new fields, the change should be easy to accommodate as it would not break existing consumers.

This solution requires both the client-side and server-side developers to work together and design APIs. But this solution is difficult to implement unless the API team is flexible and there are clear communication channels established between the API and App developers.

Most large organizations struggle with this approach and client-side developers end up bloating the code with logic which could have easily been shifted to the server. This brings us to the next approach which works best in situations where it's difficult to get the existing APIs to change.

#### Add an API facade layer

This approach is about adding a middle-layer which will be directly consumed by the client-side and it will in-turn talk to the back-end APIs. You can build this layer for multiple consumers as long as the content they consume is similar.

Though most of the time this layer will simply act as a proxy for the back-end APIs, there are cases (like the one to generate full name) where additional fields can be added to generate appropriate response.

An added benefit of this middle-layer is that it can filter out fields which are not required by it's consumers. For example, if we don't see the need for first and last name fields in the apps, they can be filtered out all-together. This helps keep the response size minimal.

Note that even if you have this middle-layer, avoid adding business logic (something that requires domain knowledge) in this layer. It's probably alright to add business logic temporarily to help release something faster. But the goal should be to push all business logic to where it belongs.

#### To Conclude

Client-side code is expensive to change. Specially for mobile apps where an update needs to be downloaded. Additionally, some users are not willing to upgrade easily, and thus you are forced to support outdated apps. Thus, keep your client-side apps as light-weight as possible and push logic to your servers.

