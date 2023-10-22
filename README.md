#Advanced-Python-Script
import requests
from bs4 import BeautifulSoup

def get_trending_repositories(language):
    url = f'https://github.com/trending/{language}'
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')

    repositories = []

    for article in soup.find_all('article'):
        repo_info = article.h1.a
        repo_name = repo_info.text.strip()
        repo_url = f"https://github.com{repo_info['href']}"
        
        description = article.p.text.strip()

        stars = article.find('a', class_='muted-link').text.strip()

        repositories.append({
            'Name': repo_name,
            'Description': description,
            'Stars': stars
        })

    return repositories

if __name__ == "__main__":
    language = input("Enter a programming language: ")
    trending_repositories = get_trending_repositories(language)
    
    print(f"Top Trending {language.capitalize()} Repositories on GitHub:")
    
    for repo in trending_repositories:
        print(f"Name: {repo['Name']}")
        print(f"Description: {repo['Description']}")
        print(f"Stars: {repo['Stars']}")
        print()

