# Supabase Node.js API

A simple, framework-less Node.js API that connects to a Supabase backend. Designed for serverless deployment on platforms like Vercel.

## Setup

1.  **Clone the repository (or download the files).**

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Set up environment variables:**
    - Create a `.env` file in the root of the project by copying the `.env.example` file:
      ```bash
      cp .env.example .env
      ```
    - Open the `.env` file and replace the placeholder values with your actual Supabase URL and anon key. You can find these in your Supabase project's API settings.



## Running Locally

To start the server for local development, run:

```bash
node api/index.js
```

The server will start on port 3000.

## API Endpoints

### Get All Records

- **Method:** `GET`
- **URL:** `/api/ctp`
- **Description:** Retrieves all records from the `ctp` table.

### Update a Record

- **Method:** `PATCH`
- **URL:** `/api/ctp/:id` (e.g., `/api/ctp/1`)
- **Description:** Updates the `licenciado` status for a specific record.
- **Request Body:** A JSON object with the new boolean value.
  ```json
  {
    "licenciado": true
  }
  ```
- **Response:** The updated record object.
- **Example Usage (JavaScript Fetch):**
  ```javascript
  /**
   * Updates the 'licenciado' status for a specific record.
   * @param {number} id - The ID of the record to update.
   * @param {boolean} isLicenciado - The new value for the 'licenciado' field.
   */
  async function updateLicenciadoStatus(id, isLicenciado) {
    // The URL of your API endpoint, including the specific ID
    const url = `http://localhost:3000/api/ctp/${id}`;

    // The data to send in the request body
    const body = {
      licenciado: isLicenciado,
    };

    try {
      const response = await fetch(url, {
        method: 'PATCH', // The HTTP method for the request
        headers: {
          'Content-Type': 'application/json', // Tells the server we are sending JSON
        },
        body: JSON.stringify(body), // Converts the JS object to a JSON string
      });

      if (!response.ok) {
        // Handle HTTP errors (like 404 Not Found or 500 Server Error)
        const errorData = await response.json();
        throw new Error(`API Error: ${response.status} - ${errorData.error || 'Unknown error'}`);
      }

      // Get the updated record from the API's response
      const updatedRecord = await response.json();
      console.log('Successfully updated record:', updatedRecord);
      
      return updatedRecord;

    } catch (error) {
      console.error('Failed to update status:', error);
      // Here you could show an error message to the user
    }
  }

  // --- Example of how to use the function ---

  // To set the 'licenciado' status of the record with ID 5 to TRUE
  // updateLicenciadoStatus(5, true);

  // To set the 'licenciado' status of the record with ID 8 to FALSE
  // updateLicenciadoStatus(8, false);
  ```

## Deployment

This project is pre-configured for deployment on [Vercel](https://vercel.com/).

1.  Push your code to a Git repository (e.g., on GitHub).
2.  Import the repository into your Vercel account.
3.  Vercel will automatically detect the `vercel.json` configuration.
4.  Add your `SUPABASE_URL` and `SUPABASE_KEY` as environment variables in the Vercel project settings.
5.  Deploy!
