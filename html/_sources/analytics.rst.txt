Analytics
____________________

To calculate analytics on a financial instrument, security, or smart contract, you may use the FinXClient client or the FinXRest client, depending on the function called.

The FinXClient is instantiated as follows:

.. code-block::
   :caption: Instantiate FinXClient

    import os
    import finx
    from finx.client import FinXClient
    finx_api_key = os.environ[FINX_API_KEY]
    if finx_api_key:
        finx_client = FinXClient("socket",ssl=True)
    else:
        print("set your FINX_API_KEY environment variable like os.environ[FINX_API_KEY] = my_api_key")

The FinXRest client is instantiated as follows:

.. code-block::
   :caption: Instantiate FinXRest

   import os
   import finx
   from finx.finx_rest import FinXRest
   finx_api_key = os.environ['FINX_API_KEY']
   finx_rest_client = FinXRest(finx_api_key)

The FinxClient and FinXRest clients expose several functions:

____________________

.. py:function:: FinXClient.list_api_functions()

    Return the list of functions that may be passed into the FinXClient.

    :param null: none
    :return: List of functions that may be used.
    :rtype: list

____________________

.. py:function:: FinXClient.calculate_greeks(s0, k, r, sigma, q, T, price, option_side, option_type)

    Calculate greeks (quant risk measures) on derivative instruments of any type.

    :param s0: Required
    :type s0: float
    :param k: Required
    :type k: float
    :param sigma: Required
    :type sigma: float
    :param q: Required
    :type q: float
    :param T: Required Days to option expiration
    :type T: int
    :param price: Price of the option
    :type price: float
    :param option_side: Optional "put" or "call"
    :type option_side: str
    :param option_type: Optional "american" or "european" or "perpetual"
    :type option_type: str
    :return: Dictionary of key:value pairs for each Greek measure with keys [vol, price, delta, gamma, theta, vega, vanna, volga, rho]
    :rtype: dict

____________________

.. py:function:: FinXClient.calculate_security_analytics(security_id, as_of_date, price, lognormal, volatility, drift_a, yield_shift, shock_in_bp, price_as_yield, cpr, psa)

    Return dictionary of analytics for the security submitted. If an as_of_date is entered the value is calculated as of that date with data known on that date.

    :param secruity_id: Required Any security_identifier of any number of id schemes
    :type security_id: str
    :param as_of_date: Optional A date formatted as "YYYY-mm-dd" (like "2022-07-01") to use as a historical look-up of reference data.
    :type as_of_date: str
    :param price: Optional A price to use for the analysis, otherwise one of [par, last best price, observation price] depending on the instrument, date selected, and API Key role
    :type price: float
    :param lognormal: Optional Whether to use "lognormal" or "normal" distribution. Default is True
    :type lognormal: binary
    :param volatility: Optional The volatility of the instrument
    :type volatility: float
    :param drift_a: Optional  The mean reversion parameter for trinomial tree calculations
    :type drift_a: float
    :param yield_shift: Optional The yield shift parameter, i.e. how much the effective duration shifts for v+/v-
    :type yield_shift: float
    :param shock_in_bp: Optional The basis point shock parameter, i.e. how much all rates used to calibrate eff duration get shocked in valuation
    :type shock_in_bp: float
    :param price_as_yield: Optional whether to use the price field entered as a Yield value. Default is False
    :type price_as_yield: binary
    :param cpr: Optional Conditional prepayment rate. If "cpr" is entered then "psa" is ignored.
    :type cpr: float
    :param psa: Optional Payment speed benchmark. If "cpr" is entered then "psa" is ignored.
    :return: List of dictionaries with cash flow details for each period forecasted.
    :rtype: list
____________________

.. py:function:: FinXClient.calculate_security_key_rates(security_id, as_of_date, price, lognormal, volatility, drift_a, yield_shift, shock_in_bp, price_as_yield, cpr, psa)

    Return dictionary of analytics for the security submitted. If an as_of_date is entered the value is calculated as of that date with data known on that date.

    :param secruity_id: Required Any security_identifier of any number of id schemes
    :type security_id: str
    :param as_of_date: Optional A date formatted as "YYYY-mm-dd" (like "2022-07-01") to use as a historical look-up of reference data.
    :type as_of_date: str
    :param price: Optional A price to use for the analysis, otherwise one of [par, last best price, observation price] depending on the instrument, date selected, and API Key role
    :type price: float
    :param lognormal: Optional Whether to use "lognormal" or "normal" distribution. Default is True
    :type lognormal: binary
    :param volatility: Optional The volatility of the instrument
    :type volatility: float
    :param drift_a: Optional  The mean reversion parameter for trinomial tree calculations
    :type drift_a: float
    :param yield_shift: Optional The yield shift parameter, i.e. how much the effective duration shifts for v+/v-
    :type yield_shift: float
    :param key_rate_times: Optional Tenor(s) of the interest rate curve for which sensitivity is calculated. For example a list of ['6mo', '1yr', '2yr', '3yr','5yr','7yr','10yr']
    :type key_rate_times: str
    :param shock_in_bp: Optional The basis point shock parameter, i.e. how much all rates used to calibrate eff duration get shocked in valuation
    :type shock_in_bp: float
    :param price_as_yield: Optional whether to use the price field entered as a Yield value. Default is False
    :type price_as_yield: binary
    :param cpr: Optional Conditional prepayment rate. If "cpr" is entered then "psa" is ignored.
    :type cpr: float
    :param psa: Optional Payment speed benchmark. If "cpr" is entered then "psa" is ignored.
    :return: List of dictionaries with cash flow details for each period forecasted.
    :rtype: list
____________________

.. py:function:: FinXClient.calibrate_tree(curve_data, num_paths, volatility, drift_a, lognormal)

    Return a calculated trinomial tree.

    :param curve_data: Required A set of curve data in a list of points.
    :type curve_data: list
    :param volatility: Optional The volatility of the instrument
    :type volatility: float
    :type volatility: float
    :param drift_a: Optional  The mean reversion parameter for trinomial tree calculations
    :type drift_a: float
    :param lognormal: Optional Whether to use "lognormal" or "normal" distribution. Default is True
    :type lognormal: binary
    :return: List of dictionaries of monte carlo paths.
    :rtype: list


____________________

.. py:function:: FinXClient.generate_monte_carlo_paths(curve_data, num_paths, volatility, drift_a, lognormal)

    Return a set of calculated monte carlo paths.

    :param curve_data: Required A set of curve data in a list of points.
    :type curve_data: list
    :param num_paths: Required Number of simulations paths to run
    :type num_paths: int
    :param volatility: Optional The volatility of the instrument
    :type volatility: float
    :type volatility: float
    :param drift_a: Optional  The mean reversion parameter for trinomial tree calculations
    :type drift_a: float
    :param lognormal: Optional Whether to use "lognormal" or "normal" distribution. Default is True
    :type lognormal: binary
    :return: List of dictionaries of monte carlo paths.
    :rtype: list


____________________

.. py:function:: FinXClient.get_curve(curve_name, currency, start_date, end_date, country_code, fixing, tenor)

    Return reference data, terms and conditions on a security based on its security_id. If an "as_of_date" is entered then the method returns data as it was known on that date. This can be used for retrieving historical data.

    :param curve_name: Required A curve name in the following list: []
    :type curve_name: str
    :param currency: Required Currency such as "USD", "EUR", "YEN"
    :type currency: str
    :param start_date: Required Start date of curve as string in format "YYYY-mm-dd" such as "2022-07-01"
    :type start_date: str
    :param end_date: Optional End date of curve as string in format "YYYY-mm-dd" such as "2022-07-01"
    :type end_date: str
    :param country_code: Optional Country code such as "USA", "GBR", "JPN"
    :type country_code: str
    :param fixing: Optional Fixing rate for floating leg index
    :type fixing: float
    :param tenor: Optional Remaining time until maturity in years
    :type tenor: float
    :return: Dictionary of key:value pairs for the security. Keys will vary based on security_type, but typical keys are [security_name, asset_class, security_type, data_source, country, currency, current_factor, maturity_date, issue_date, current_coupon, etc.]
    :rtype: dict

____________________

.. py:function:: FinXClient.get_security_cash_flows(security_id, as_of_date, price, lognormal, volatility, drift_a, yield_shift, price_as_yield)

    Return List of dictionaries of FORECASTED cash flows for periods from the as_of_date forward until maturity date. This method is used for BONDS, STRUCTURED PRODUCTS and other "FIXED INCOME" securities.

    :param secruity_id: Required Any security_identifier of any number of id schemes
    :type security_id: str
    :param as_of_date: Optional A date formatted as "YYYY-mm-dd" (like "2022-07-01") to use as a historical look-up of reference data.
    :type as_of_date: str
    :param price: Optional A price to use for the analysis, otherwise one of [par, last best price, observation price] depending on the instrument, date selected, and API Key role
    :type price: float
    :param lognormal: Optional Whether to use "lognormal" or "normal" distribution. Default is True
    :type lognormal: binary
    :param volatility: Optional The volatility of the instrument
    :type volatility: float
    :param drift_a: Optional  The mean reversion parameter for trinomial tree calculations
    :type drift_a: float
    :param yield_shift: Optional The yield shift parameter, i.e. how much the effective duration shifts for v+/v-
    :type yield_shift: float
    :param price_as_yield: Optional whether to use the price field entered as a Yield value. Default is False
    :type price_as_yield: binary
    :return: List of dictionaries with cash flow details for each period forecasted.
    :rtype: list



