const axios = require('axios');
const cheerio = require('cheerio');

const scrapeArticles = async () => {
    try {
        // Fetch the HTML content of the website
        const response = await axios.get('http://time.com/');
        
        // Load the HTML content into Cheerio
        const $ = cheerio.load(response.data);
        
        // Select the elements containing the articles
        const articles = $('.latest-stories__item');
        
        // Iterate over each article and extract relevant information
        const articleData = articles.map((index, element) => {
            const title = $(element).find('.latest-stories__item-headline').text().trim();
            const link = 'https://time.com' + $(element).find('.latest-stories__item a').attr('href');
            return { title, link };
        }).get();
        
        console.log(articleData);
    } catch (error) {
        console.error('Error fetching articles:', error);
    }
};

scrapeArticles();
