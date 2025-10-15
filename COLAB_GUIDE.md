# Guide: Upload to GitHub and Access from Google Colab

## Step 1: Create a Public GitHub Repository

1. Go to [GitHub](https://github.com) and log in
2. Click the "+" icon in the top right corner
3. Select "New repository"
4. Fill in the details:
   - **Repository name**: `AirbnbData` (or any name you prefer)
   - **Description**: "Airbnb listings dataset"
   - **Visibility**: Select **Public** (important for direct access from Colab)
   - **DO NOT** initialize with README, .gitignore, or license (we already have these files)
5. Click "Create repository"

## Step 2: Push Your Code to GitHub

After creating the repository, GitHub will show you commands. Run these in your terminal:

```bash
cd /Users/matteomainetti/Desktop/AirbnbData
git remote add origin https://github.com/YOUR_USERNAME/AirbnbData.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your actual GitHub username.

**Note**: You may need to authenticate. GitHub requires a Personal Access Token (PAT) instead of a password:
- Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
- Generate a new token with `repo` permissions
- Use this token as your password when prompted

## Step 3: Get the Raw Data URL

Once pushed to GitHub:
1. Navigate to your repository on GitHub
2. Click on `listings.csv`
3. Click the "Raw" button
4. Copy the URL from your browser's address bar
5. It should look like: `https://raw.githubusercontent.com/YOUR_USERNAME/AirbnbData/main/listings.csv`

## Step 4: Access Data from Google Colab

### Option 1: Direct URL Loading (Recommended for Public Repos)

Open a new [Google Colab notebook](https://colab.research.google.com/) and use this code:

```python
import pandas as pd

# Replace with your actual GitHub username
GITHUB_USERNAME = "YOUR_USERNAME"
REPO_NAME = "AirbnbData"

# Load the data directly from raw GitHub URL
url = f'https://raw.githubusercontent.com/{GITHUB_USERNAME}/{REPO_NAME}/main/listings.csv'
df = pd.read_csv(url)

# Display basic information
print(f"Dataset loaded successfully!")
print(f"Shape: {df.shape}")
print(f"Columns: {df.columns.tolist()}")
print("\nFirst few rows:")
df.head()
```

### Option 2: Clone the Repository (If you need all files)

```python
# Clone the repository
!git clone https://github.com/YOUR_USERNAME/AirbnbData.git

# Load the data
import pandas as pd
df = pd.read_csv('AirbnbData/listings.csv')

# Display information
print(f"Dataset shape: {df.shape}")
df.head()
```

### Option 3: Using PyGithub (For more complex operations)

```python
# Install PyGithub if needed
!pip install PyGithub

from github import Github
import pandas as pd
import io

# For public repositories, no token needed
g = Github()

# Get the repository
repo = g.get_repo("YOUR_USERNAME/AirbnbData")

# Get the file content
file_content = repo.get_contents("listings.csv")

# Decode and load into pandas
import base64
decoded_content = base64.b64decode(file_content.content)
df = pd.read_csv(io.BytesIO(decoded_content))

print(f"Dataset shape: {df.shape}")
df.head()
```

## Complete Example Colab Notebook

Here's a complete example you can copy-paste into a new Colab notebook:

```python
# =============================================================================
# Load Airbnb Data from GitHub
# =============================================================================

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Configuration
GITHUB_USERNAME = "YOUR_USERNAME"  # Replace with your GitHub username
REPO_NAME = "AirbnbData"

# Load the data
print("Loading data from GitHub...")
url = f'https://raw.githubusercontent.com/{GITHUB_USERNAME}/{REPO_NAME}/main/listings.csv'
df = pd.read_csv(url)

print("✓ Data loaded successfully!")
print(f"\nDataset Information:")
print(f"  - Shape: {df.shape[0]} rows × {df.shape[1]} columns")
print(f"  - Memory usage: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")

# Display first few rows
print("\nFirst 5 rows:")
display(df.head())

# Display column information
print("\nColumn Information:")
display(df.info())

# Display basic statistics
print("\nBasic Statistics:")
display(df.describe())

# Check for missing values
print("\nMissing Values:")
missing = df.isnull().sum()
missing = missing[missing > 0].sort_values(ascending=False)
if len(missing) > 0:
    display(missing)
else:
    print("No missing values found!")
```

## Troubleshooting

### Issue: "Repository not found" or "404 error"
- Ensure the repository is set to **Public** (not Private)
- Double-check your username and repository name spelling
- Wait a few seconds after pushing for GitHub to index the files

### Issue: "File too large" warning from GitHub
- GitHub has a 100 MB file size limit for web interface
- For files 50-100 MB, they'll work but with warnings
- For files > 100 MB, consider using Git LFS or alternative hosting

### Issue: Authentication required when pushing
- Use a Personal Access Token instead of password
- Or set up SSH keys for GitHub authentication

## Tips

1. **Keep your data updated**: Whenever you update `listings.csv`, commit and push the changes. Colab will automatically fetch the latest version.

2. **Version control**: Use git tags to version your datasets:
   ```bash
   git tag -a v1.0 -m "Initial dataset version"
   git push origin v1.0
   ```
   Then access specific versions in Colab using tags in the URL.

3. **Large datasets**: If your CSV is very large (>100 MB), consider:
   - Using Git LFS (Large File Storage)
   - Splitting into multiple smaller files
   - Hosting on a different service (Google Drive, Kaggle, AWS S3)

4. **Private repositories**: If you make the repo private, you'll need to use authentication tokens in your Colab notebook.

---

Happy analyzing! 🚀

