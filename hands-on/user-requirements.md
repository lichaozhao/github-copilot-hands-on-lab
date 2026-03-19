
# User Requests

## CRUD 
```
1. Allow users to remove books from their favorites list.
2. Add a "Clear All Favorites" button that allows users to remove all books from their favorites list at once.
```
## UI/UE Requests
3. Implement sorting options for the book list to allow users to sort by title or author.
4. Add a search functionality to the book list that allows users to search by title or author in real-time.


## Data Model enchancements 
5. Allow different types of users: member and administrators. The user type will have to be displayed close to the user name in the header of the application.
6. Can you add a new feature that allows users to add a comment on a favorite?
7. Can you update #file:books.json (without any script) to add the written date and a description by using the information through web search?
8. Implement a book review system by breaking down the feature into multiple sub-tasks and using a plan to coordinate the implementation.
9. Add a feature to categorize books into different genres or categories.
10. Multiple language support.



## Advanced Features
11. Add a chat bot that can answer questions about the books in the list.
12. Add end-to-end tests for the entire application to ensure all features work together seamlessly.



# Suggested Prompt Examples 
## for request #3:

```prompt
   Title: Add book list sorting options

   Description:
   As a user, I want to be able to sort the book list by different criteria (title, author) to better organize and find books.

   Requirements:
   - Add sorting options (dropdown or buttons) for title and author
   - Implement sorting on both frontend and backend
   - Maintain sort state when navigating
   - Add visual indication of current sort order
   - Include unit and integration tests
```
## for request #4:
   ```prompt
   I want to add a search feature to the book list that filters books by title or author.
   The search should work in real-time as the user types.

   Help me create a SearchInput component that:
   - Has a clean, modern design
   - Shows a search icon
   - Has a clear button
   - Updates in real-time

   Help me implement the filter logic to:
   - Search in both title and author fields
   - Be case-insensitive
   - Handle special characters
   - Update the list in real-time

   Help me integrate the search state with Redux:
   - Add search term to the store
   - Update the book list selector
   - Persist search state during navigation

   Help me write tests for:
   - The SearchInput component
   - The filter logic
   ```

## for request #8:
```prompt
   Title: Frontend Implementation - Book Reviews UI Components

   Description:
   Implement the frontend components for the book review system.

   Requirements:
   - Add a "Reviews" section to each book card
   - Create a form for submitting new reviews with:
     * Rating (1-5 stars)
     * Review text
     * Submit button
   - Display existing reviews in a scrollable list
   - Show average rating
   - Add loading states and error handling
   - Include frontend unit tests

   Technical Considerations:
   - Use existing styling patterns
   - Implement proper form validation
   - Consider responsive design
```

```prompt 
   Title: Backend Implementation - Book Reviews API

   Description:
   Implement the backend API and database changes for the book review system.

   Requirements:
   - Create new database schema for reviews
   - Implement REST API endpoints:
     * POST /api/books/{id}/reviews
     * GET /api/books/{id}/reviews
     * GET /api/books/{id}/average-rating
   - Add input validation
   - Implement error handling
   - Add backend unit tests
   - Update API documentation

   Technical Considerations:
   - Implement proper validation middleware
   - Add rate limiting for review submission
```

```prompt
   Title: Implement Book Review System

   Description:
   Add the ability for users to review books and see others' reviews.

   Requirements:
   This feature consists of two parts:
   - Frontend implementation: #[Link to Frontend prompt]
   - Backend implementation: #[Link to Backend prompt]
```



## for request #9:

```prompt
   I want to add a book categories feature

   can you help me write a detailed description for implementing book categories in our application?
     - Technical requirements
     - UI/UX considerations
     - Testing requirements
     - Acceptance criteria

   Don't forget to add tests.
```