# CASE 1 - ELECTRICITY PRICE FORECASTING
![case1](https://github.com/Eik-Lab/NBIM-hackathon/assets/91673370/819d8d33-4b45-4052-8cd3-dca9c4cb2ee7)

## BACKGROUND
As the global transition to renewable energy gathers pace, individual power generation is becoming an increasingly viable option. Many homeowners and small-scale entrepreneurs are installing solar panels, wind turbines, and battery storage systems. This decentralized energy generation model has the potential to reshape energy markets, not only from an infrastructural standpoint but also from a financial trading perspective, especially for investment banks that are always on the lookout for emerging market trends. 

Individuals feeding excess power back into the grid present both a challenge and an opportunity. For investment banks, accurately forecasting these feed-in times and quantities is paramount. It aids them in creating financial products, derivatives, and trading strategies tailored to this new energy landscape. The crucial question, therefore, is: When is the optimal time for these individual power providers to reintegrate energy for maximum financial return, both for themselves and for the broader financial market? 

Emerging technologies are now empowering even lay consumers with insights into the determinants of optimal energy feed-in timings. Such a transparent approach can significantly benefit both individual power producers and the larger grid in terms of efficiency. However, for investment banks, this clarity is doubly beneficial. It allows them to manage risks, devise new financial instruments, and offer guidance to their clientele about potential investment opportunities in this domain. Still, the industry's intricate terminology and multiple influencing factors can convolute this understanding. 

The challenge becomes multi-dimensional: Firstly, can we leverage diverse technological tools, like data models and satellite imagery, to demystify real-time and forecasted grid needs? Secondly, can this information be relayed in a straightforward manner to individual power generators, ensuring they make well-informed decisions? Lastly, how can investment banks harness this data to innovate their trading strategies and financial offerings? 

In the context of a rapidly evolving energy and financial landscape, how can we devise a transparent forecasting model that not only guides individual power providers on optimal feed-in times but also aids investment banks in navigating the complexities and opportunities of decentralized energy trading? 


## GOAL

Design a predictive algorithm that determines optimal energy feed-in timings for individual power producers, enabling investment banks to better assess the financial implications and market dynamics of increased decentralized energy contributions to the grid.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/37374275/39667a9f-6e39-468b-a38d-af27bc52ff55)

### SUGGESTED APPROACH

- Technology exploration
  - Explore Transformer-based Timeseries predictions
  - Find available datasources within Norwegian market
    - Historical weather from met.no
- Project Specification
  - Define the requirements of the system.
- System Architecture
  - Describe an overview of how a system can be built.
- Development of Minimum Viable Concept

### Overview of key data and tools

- https://wegaw.com/swiss-industry-and-science-consortium-on-track-to-optimise-hydropower-production-using-satellite-data/
- https://business.esa.int/projects/defrost-for-hydropower
- https://www.nature.com/articles/s41597-023-02008-2
- https://huggingface.co/docs/transformers/model_doc/time_series_transformer

#### Open datasets

- https://www.kaggle.com/datasets?search=electricity+price
- https://www.kaggle.com/code/dimitriosroussis/electricity-price-forecasting-with-dnns-eda
