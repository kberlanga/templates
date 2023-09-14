# [Project Name] [Backend | Frontend] 
## Packages

* [Library Reference](https://url/) - A minor description about the library

## [Programming environment] version

To run this project, you will need to use the [Programming environment] version:

`vx.x.x`

If you have nvm (node version manager or similar) installed on your computer, you just run the following command (or similar):

```bash
nvm use
```

you will be using the right node version.

## Installation

To install all packages needed to run this app use

```bash
npm install
```

## Run the app

To run this project, you will need to add the following environment variables to the .env file (if the file does not exist, create it)

```bash
ENVIRONMENT_VAR =
```

_You should find this template in the file `.env.template`_

`URL_VAR` is the API URL regarding the environment where you want the app to run. You can find the API URLs based on the environments in the next section.

The administrator of the Pusher and Google Recaptcha accounts must provide the keys.

### Environments

| Environment | API URL |
|-------------|------------------|
| local | http://localhost:3000 |
| development | https://dev.api |
| staging | https://staging.api |
| production | https://api |

### Start app
After adding the variables required in the `.env` file, you can run the following command to run your app locally:

```bash
npm run start
```

Your app should run in [http://localhost:3000](http://localhost:3000)

If you want to run your application using the local API (after running the local API), you have to add the API URL of the environment `local` (check the previous table) and then run the following command:

```bash
npm run start:local
```

Your app should run in [http://localhost:3001](http://localhost:3001) and your API should run in [http://localhost:3000](http://localhost:3000)

## Running Tests

### Component Tests

To run tests, run the following command

```bash
npm run test
```

To run test coverage, run the following command

```bash
npm run coverage
```

### e2e Tests

To run e2e tests, run the following command

```bash
npm run cypress:open
```

## Running Docker

To run this app from Docker, run the following command

```bash
docker-compose up -d
```

or

```bash
docker-compose up -d
```

## Project Architecture

One of the most important aspects of the frontend architecture is `componentization`. Breaking down the application into small reusable components allows for better organization, separation of concerns, and ease of maintenance.

### Directory Structure
The top-level directory structure will be as follows:

```
.
└── /src
├── /components
├── /context
├── /hooks
├── /i18n
├── /layouts
├── /mocks
├── /modals
├── /models
├── /pages
├── /routes
├── /services
├── /styles
├── /theme
├── /utils
├── index.tsx
├── AppProviders.tsx
├── index.css
└── setupTests.ts
```

* **components** - This folder contains global shared/reusable components, such as wrappers, form components, etc
* **context** - This folder stores all React context files that are used across multiple pages
* **hooks** - This folder contains every single custom hook
* **i18n** - This folder contains the internationalization and localization properties files
* **layouts** - This is just a special folder for placing any layout-based components. This would be things like a sidebar, navbar, container, etc
* **mocks** - This folder contains setup files for
* **modals** - This folder contains all the components related to the application's modals
* **models** - This folder contains the structures/models used in the application
* **pages** - This folder contains each page in the application
* **routes** - This folder contains all related to routing
* **services** - This folder contains all your code for interfacing with any external API
* **styles** - This folder contains global styles and animations
* **theme** - This folder contains the setup file for styling components
* **utils** - This folder is for storing all utility functions such as formatters

### Create a new page from scratch

If your page has modules is recommended to create small components to maintain the architecture of the project.

So, you have to follow these steps to create a new page:

1. Create a new route in `src/routes/routes.ts`. You have to add the path and the allowed roles for this path:

* If your page is a `protected route` (protected means that your page requires that the user logs in to the application)

```ts
    NEW_ROUTE: {
        path: "/new-route",
        allowedRoles: [ROLES.ADMIN, ROLES.PROVIDER, ROLES.ASSISTANT, ROLES.SUPER],
    },
```

* If your page is an `unprotected route` (unprotected means that your page does not require that the user log in to the application)

```ts
    NEW_ROUTE: {
        path: "/new-route",
        allowedRoles: null
    }
```
2. Create the folder into `src/pages, the name’s folder should be the name of your new page

```ts
    export function SignUp() {
        return <p> new page works! </p>
    }
```

3. Now you have to add a page component into the `src/routes/router.ts`:

* If your new page is protected you will add a new object to the ROUTER const

```ts
    {
        element: <ProtectedLayout allowedRoles={ROUTES.NEW_ROUTER.allowedRoles!} />,
        children: [
                {
                    path: ROUTES.NEW_ROUTER.path,
                    element: <NewPage />,
                },
            ],
    }
```

* If your new page is unprotected, you will add a new object to the UnprotectedLayout children array:
```ts
    {
        path: getRelativeRoute(ROUTES.NEW_ROUTER.path),
        element: <NewPage />,
    }
```

4. Now, if you go to the path of your new page, you will see “new page works” in your browser

### How to consume the API from app

In the file [src/services/http/endpoints.ts](https://github.com/ultrasound-dot-ai/ultrasound_ai/blob/main/src/services/http/endpoints.ts) you will find async functions for each HTTP method (GET, POST, PUT, PATCH, DELETE), these function will help to you to implement the endpoints.

For example, we suppose you want to get the patient list in the new page that you created in the previous section. You will have to use the endpoint `httpGet<T>` to get the patients.

_You will find the API documentation in this [repository](https://github.com/ultrasound-dot-ai/ultrasound_ai_api)_.

Example:
* Endpoint: GET /institution/panel
* Interface Response: PatientResponse
* Params: { search: "Alisson" }

```ts
    httpGet<PatientResponse>({ path: "/institution/panel", filters: {search: "Alisson" } })
    .then((response) => {
    console.log(response)
    })
    .catch((error: AxiosError) => {
    console.log(error)
    });
```

# Contribute

You will find more information about how to contribute to this repository [here](https://github.com/ultrasound-dot-ai/ultrasound_ai/blob/main/CONTRIBUTING.md).
