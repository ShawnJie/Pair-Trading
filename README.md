# Pair-Trading
Pair Trading in Fixed Income Market

In this project, I designed and implemented a pair trading system for interest rate derivatives. I screened pairs among an instrument universe of Eurodollar future, American treasury bond and Canadian bond using multiple criteria. Then I optimized trading rules and tested the system. Out-of-sample test gave me a not good performance, but it’s still a good practice.

Instruments universe includes 3-month Eurodollar Contract: ED1, ED2, …, ED19, ED20, US treasury bond: TU (2-year), FV (5-year), TY (10-year), US (30-year), UL (Ultra), Canadian bond: CGZ (2-year), CGF (5-year), CGB (10-year). 
 We collect data from 2013 till now from Quandl and split it into two groups. Training group will start from 2013 and end at 2017. Test group is 2018 till now.

We develop a 3 stage multiple criteria screening rules for pair screening. We screening pairs using 4 criteria, correlation, cointegration, beta and volatility. Firstly, we calculate correlations between all instruments and only move those pairs with correlation over 0.8 to next stage. Then we do TLS regression to log prices of these pairs and only select pairs whose P value lower than 0.05 (95% significance level). These two stages are common criteria. Next stage is practical here. We require those pairs regression coefficients to be lower than 3 and volatility greater than 0.001. The reason is that when beta is greater than 3, the regression can be very non-stationary. A very little change can result in regression failure. For volatility, our logic is we need volatility of residuals to be high enough for us to make profit. Otherwise, we will trade very frequently but make very little profit each time. When considering transaction costs, this situation can be a serious problem. However, in this project, our pairs cointegrated so well that all residuals volatility is very small. We can only set a level of 0.001 as our criteria.

For trading rules optimization, we answer 5 questions. 
How should we allocate capital among selected pairs?
When should we open a position?
When should we close a position?
When should we cut a position?
How do transaction costs influence our result?
For the first question, we haves to ways to select. Firstly, the very simplest method is allocating them equally among all pairs. Secondly, we allocate them proportional to the dollar volatility. We tried both ways and results showed that the second one is better. 
Then we set up rules for opening, closing and cutting a position. We open a position when residuals cross α_1*vol. We close a position when residuals go back to α_2*vol. We cut a position when residuals reach α_3*vol. Notice our pair trading based on the mean reversion of residuals. So theoretically, we have α_2< α_1< α_3. To decide these three parameters, we run our in-sample portfolio with different parameters combination. We choose parameters by comparing Pnl, trade frequency and hit ratio. Basically we want our Pnl and hit ratio to be high and trade frequency in a reasonable range. On the one hand, we don’t want to trade too frequently, resulting in low profit and high costs. On the other hand, wo don’t want to trade too unfrequently, with each trade a very high profit. That makes us burden too much risk. 
To see the influence of transaction costs, we decided transaction cost to be 7 $/trade + 0.00221 $/share. The cost rule is something used in U.S. stock market. 

We find transaction cost has little influence in our project, reducing profit by less than 5%. And it doesn’t influence our choice of parameters. To be closer to reality, we decide to do this project with transaction cost. We choose our parameters combination to be (1, 0, 4), which gives us higher Pnl and hit ratio with reasonable frequency.

We use these rules to test out of sample. Performance is bad. The reason for bad performance is cointegration failure out of sample. Why cointegration fails? I think the reason is in sample cointegration result is too good and volatility is too small. When it goes out of sample. A VERY LITTLE deviation can change whole story.
