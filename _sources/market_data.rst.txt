Market Data
____________________

To retrieve Market Data on a financial instrument, security, or smart contract, you may use the FinXClient or the FinXRest client, depending on the function called.

The FinXRest client is instantiated as follows:

.. code-block::
   :caption: Instantiate FinXRest

   import os
   import finx
   from finx.finx_rest import FinXRest
   finx_api_key = os.environ['FINX_API_KEY']
   if finx_api_key:
       finx_rest_client = FinXRest(finx_api_key)
   else:
       print("set your FINX_API_KEY environment variable like os.environ[FINX_API_KEY] = my_api_key")


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

The FinxClient and FinXRest clients expose several functions:

____________________

.. py:function:: FinXRest.get_reference_data(security_id, unix_time)

    For *DIGITAL ASSETS AND DIGITAL DERIVATIVES*:  Return information about a security based on its security_id. If unix_time is included then information about the
    security will be returned for the corresponding time entered. If the security information is not available for the
    time entered then either Current Information or No Information will be returned.

    :param security_id: Required "security identifier" of the item searched. This can be any industry-standard identifier or a custom identifier
    :type security_id: str
    :param unix_time: Optional "unix time" as a string(integer) of seconds since 01 January 1970
    :type unix_time: str
    :return: Depending on the security, information such as name, country, currency, security type, maturity date, etc.
    :rtype: dict

____________________

.. py:function:: FinXClient.get_security_reference_data(security_id, as_of_date)

    For *BONDS, STRUCTURED SECURITIES, and other 'FIXED INCOME' PRODUCTS*:Return reference data, terms and conditions on a security based on its security_id. If an "as_of_date" is entered then the method returns data as it was known on that date. This can be used for retrieving historical data.

    :param secruity_id: Required Any security_identifier of any number of id schemes
    :type security_id: str
    :param as_of_date: Optional A date formatted as "YYYY-mm-dd" (like "2022-07-01") to use as a historical look-up of reference data.
    :type as_of_date: str
    :return: Dictionary of key:value pairs for the security. Keys will vary based on security_type, but typical keys are [security_name, asset_class, security_type, data_source, country, currency, current_factor, maturity_date, issue_date, current_coupon, etc.]
    :rtype: dict

____________________

.. py:function:: FinXRest.list_deribit_contracts()

    Return the list of contract identifiers available on the Deribit exchange. These items may be used as security_id
    in order to retrieve additional information from other methods in this section.

    :param null:
    :return: List of security_id of Deribit contracts currently being traded.
    :rtype: list

____________________

.. py:function:: FinXRest.list_pairs()

    Return the list of pairs (swaps) tracked on the FinX Platform. You can retrieve tick-level data for any pair.
    These pairs may be used as "pair" in order to retrieve additional information from other methods in this section.

    :param null:
    :return: List of pairs being tracked.
    :rtype: list

____________________

.. py:function:: FinXRest.pair_quote(pair, unix_time_target, time_target_width)

    Return a single exchange trade and price data quote for a given pair (or "swap").

    :param pair: Required "pair" of the swap being quoted. This can be any pair of security_id separated by a ':' (colon), such as BTC:EUR or ETH:USDT.
    :type pair: str
    :param unix_time_target: Optional "unix time" (in seconds) used to search for historical data. For example, "2022-07-01 00:00:00 GMT" should be entered as "1656658800".
    :type unix_time_target: str
    :param time_target_width: Optional "width in seconds" of the time to search *around* the unix_time_target. For example, if you entered unix_time_target="1656658800" and time_target_width="20" then the pair quote will be any quote found between "2022-07-01 11:59:50 GMT" and "2022-07-02 00:00:10 GMT".
    :type time_target_width: str
    :return: Dictionary of information about the pair_quote such as "price" and "UTCDate"
    :rtype: dict

____________________

.. py:function:: FinXRest.pair_quote_series(pair, unix_time_start, step_length, unix_time_end)

    Return a time series of exchange trade and price data quotes for a given pair (or "swap"). The data returned will be between the two dates specified (unix_time_start and unix_time_end) with quotes separated in time by the step_length in seconds. The maximum number of records returned from this method is 1,000.

    :param pair: Required "pair" of the swap being quoted. This can be any pair of security_id separated by a ':' (colon), such as BTC:EUR or ETH:USDT.
    :type pair: str
    :param unix_time_start: Required "unix time start" (in seconds) used to start searching for historical data. For example, "2022-07-01 00:00:00 GMT" should be entered as "1656658800".
    :type unix_time_start: str
    :param step_length: Optional "step length" (in seconds) of the amount of time that should separate each quote. For example, "60" will return historical quotes that are 60 seconds apart.
    :type step_length: str
    :param unix_time_end: Optional "unix time end" (in seconds) used to stop searching for historical data.
    :type time_target_width: str
    :return: List of dictionaries of information about historical pair_quotes such as "price" and "UTCDate"
    :rtype: list


____________________

.. py:function:: FinXRest.get_options_timeslice(timeslice_datestamp, timeslice_width_seconds, underlying_symbol)

    Return a timeslice of options observations for a given underlying_symbol. The returned set will include all futures, options and perpetuals for a given underlying security for the specific date and time input, and including the range of time around that target time.

    :param timeslice_datestamp: Required "unix time" (in seconds) used as the center point for the timeslice of observations. For example, "2022-07-01 00:00:00 GMT" should be entered as "1656658800".
    :type timeslice_datestamp: str
    :param timeslice_width_seconds: Required "time width" (in seconds) of the amount of time around the center point that should be searched. For example, "60" will return historical quotes that are 60 seconds apart.
    :type timeslice_width_seconds: str
    :param underlying_symbol: Required "underlying symbol" of the contracts to be included in the timeslice. For example, "BTC" or "ETH".
    :type underlying_symbol: str
    :return: List of dictionaries of information about observed option contracts in the timeslice, with information such as "expiry", "strike price", "option type", "gamma", "delta", "rho", etc.
    :rtype: list

____________________

.. py:function:: FinXRest.get_options_timeslice_series(unix_time_start, unix_time_end, timeslice_width_seconds, underlying_symbol)

    Return a time series of timeslices of options observations for a given underlying_symbol, evenly distributed betwen a start and an end date. A maximum of 1,000 time slices will be returned.

    :param unix_time_start: Required "unix time" (in seconds) used as the starting time for the series of timeslices of observations. For example, "2022-07-01 00:00:00 GMT" should be entered as "1656658800".
    :type unix_time_start: str
    :param unix_time_end: Required "unix time" (in seconds) used as the ending point for the series of timeslices of observations.
    :type unix_time_end: str
    :param timeslice_width_seconds: Required "time width" (in seconds) of the amount of time around the center point that should be searched. For example, "60" will return historical quotes that are 60 seconds apart.
    :type timeslice_width_seconds: str
    :param underlying_symbol: Required "underlying symbol" of the contracts to be included in the timeslice. For example, "BTC" or "ETH".
    :type underlying_symbol: str
    :return: List of dictionaries of information about observed option contracts in the timeslice, with information such as "expiry", "strike price", "option type", "gamma", "delta", "rho", etc.
    :rtype: list


coverage_check
