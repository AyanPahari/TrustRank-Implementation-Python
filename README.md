
# TrustRank Implementation(Python)

Iterative Implementation of the TrustRank algorithm in python. 
Taken reference from https://www.vldb.org/conf/2004/RS15P3.PDF

Input

    1. List of edges
    2. Some good and bad nodes
    3. Damping factor beta

Output

    1. Transition matrix
    2. Final TrustRank vector after convergence


### TrustRank Algorithm Overview:
TrustRank is an algorithm that is used to segregate useful web pages from spam and also helps the search engines to rank pages. It helps in examining the quality of a web page.
The basic principle behind the trust rank is the notion of approximate isolation where the idea is that it's rare for a good page to point to a bad page i.e., it's rare that a legitimate/genuine page would point to a spam page. So we want to select a set of seed pages from the web that we trust (good) and then we would want to compute the similarity, importance, or proximity of all the other web pages on the web to this trusted page.
If some website is legitimate, then it will be close to the trusted set of web pages. We have an Oracle (human) to identify a good trustworthy set of web pages. But it is very hard for a human to go through millions of web pages and identify trusted pages. So, these trusted pages have to be kept to a minimum. We usually consider webpages with .edu, .gov, etc as trusted pages since it is linked with a trusted authority.
So the idea is we would have to do trust propagation.

We have a subset of good seed pages that we identify as good as we will call them trusted pages and a subset of bad pages that we identify as bad and are not trustworthy.
We will be propagating trust across the links of the graph and the trust can take a value between zero and one.
For the initial seed we will allocate the scores as follows:

    Good Pages: 1
    Bad Pages : 0
    Others: 1/2
TrustRank selects seeds by applying inverse Page Rank to the dataset. By this method, we get pages that would be most useful to identify additional pages. Another simple way of finding trust is to get pages with a high Page Rank as a high-ranked page would most probably point to another high PageRank page, this way, it will also propagate trust more. After convergence, the results are then ranked in descending order and the good pages from the top K pages are marked as good seed sets.
We can also select the seed set based on domain knowledge.

#### Mathematical Formulation:

    r = β . T . r + (1 - β) . d
    where,
    r = TrustRank Scores
    β = Damping factor
    T = Transition matrix
    d = Score Distribution vector which sums to 1. We assign special scores to good, bad, and neutral pages and the score of such pages is then spread during the iterations to the pages they point to.

#### Pseudocode of the Algorithm(used in the implementation):
    Function TrustRank
    Input
    T – Transition matrix
    N – Number of pages
    β – Damping factor(default 0.85)
    d = Score Distribution vector
    Output
    t – TrustRank Scores
    Begin
        1. Randomly initialized 50 nodes as good and 50 nodes as bad and the rest being neutral
        Scoring 1 for good pages, 0 for bad pages, and 0.5 for neutral pages and random shuffled the set. That will give us the vector d.
        v = good * [1] + bad * [0] + (N - good - bad) * [0.5]
        random.shuffle(v)
        2. Normalizing the vector d to have sum = 1
        d = d/np.sum(d)
        3. t = d
        Loop until the vector t converges
            t = β * np.dot(T,t) + (1 - β) * d
        return t
    End
#### The characteristics which are influential while finding trust in a web page are:
    1. The older the website, the more trust is placed on it. So, domain age is important.

    2. If the site is updated regularly, it indicates that it is an active site and hence trustworthy.

    3. If the website has a unique IP address, it also makes sure that this IP is not shared with some bad neighbors.

    4. Backlinks: Websites interlink with each other. There are 3 types of backlinks:
        a. Bad Links: These links are from banned websites and websites that operate Black Hat optimizations.
        b. Good links: These links are inside the body of the text and redirect to similar websites.
        c. Doubtful links: These links may be backlinks from off-topic websites.

    5. If the site doesn’t have any bad topics which mean it is associated with spam or topics like alcohol, gambling, or anything illegal.

    6. Sites that have poor up-time lose trust.

    7. Including sitemaps in a website is also considered good.



## Dataset

    graph.txt
## Screenshots

![Output Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


## Acknowledgements

 - [Research Paper used for reference](https://www.vldb.org/conf/2004/RS15P3.PDF)

