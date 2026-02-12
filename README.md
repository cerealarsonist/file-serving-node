# File Upload Functionality - Documentation

## Overview
This document explains the changes made to implement file upload functionality in the Node.js file server.

## Changes Made

### 1. server.js
**Purpose:** Updated the server to handle file upload requests.

**Key Changes:**
- Added `multer` middleware for handling multipart/form-data
- Configured storage to save files in the `uploads/` directory with unique filenames (timestamp + random number + original name)
- Added POST `/upload` route handler
- Implemented error handling for:
  - Multer errors (file size limits, etc.)
  - General errors
  - Missing files
- Added fallback content-type for unknown file types

**How it works:**
```javascript
// Multer configuration
const storage = multer.diskStorage({
    destination: './uploads/',  // Save location
    filename: (req, file, cb) => {
        // Unique filename: timestamp-random-originalname
        cb(null, Date.now() + '-' + Math.round(Math.random() * 1E9) + '-' + file.originalname);
    }
});
```

### 2. public/index.html
**Purpose:** Added a user-friendly file upload interface.

**Key Changes:**
- Added CSS styling for the upload form and status messages
- Created a form with `enctype="multipart/form-data"` for file uploads
- Added JavaScript to handle form submission via fetch API
- Implemented success/error message display
- Added visual feedback during upload process

**Form Structure:**
```html
<form id="uploadForm" enctype="multipart/form-data">
    <input type="file" name="file" required>
    <button type="submit">Upload File</button>
</form>
```

### 3. uploads/ directory
- Created to store uploaded files
- Files are named with unique identifiers to prevent overwrites

### 4. package.json
- Added `multer` as a dependency

## Usage

1. Start the server:
   ```bash
   node server.js
   ```

2. Open browser at `http://localhost:3000`

3. Select a file using the upload form and click "Upload File"

4. Uploaded files appear in the `uploads/` directory with unique names

## API Endpoint

**POST `/upload`**
- Content-Type: `multipart/form-data`
- Field name: `file`
- Response: JSON with `message` and `filename` on success, or `error` on failure

## Example Response
**Success:**
```json
{
    "message": "File uploaded successfully",
    "filename": "1699999999-123456789-image.png"
}
```

**Error:**
```json
{
    "error": "Error message here"
}
```

