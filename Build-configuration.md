**Updates to this public wiki page should be reflected on its [private equivalent][1]**

A number of build parameters are passed via a `.env` configuration file in the `brave-core` repository root directory. The following are used in official release builds:

* All platforms
  * binance_client_id
  * bitflyer_production_client_id
  * bitflyer_production_client_secret
  * bitflyer_production_fee_address
  * bitflyer_production_url
  * bitflyer_sandbox_client_id
  * bitflyer_sandbox_client_secret
  * bitflyer_sandbox_fee_address
  * bitflyer_sandbox_url
  * brave_ai_chat_endpoint
  * brave_google_api_endpoint
  * brave_google_api_key
  * brave_infura_project_id
  * brave_pref_hash_seed
  * brave_referrals_api_key
  * brave_services_key
  * brave_stats_api_key
  * brave_stats_updater_url
  * brave_sync_endpoint
  * brave_variations_server_url
  * brave_zero_ex_api_key
  * dcheck_always_on (set to 1 in PRs on non-Windows platforms)
  * gemini_production_api_url
  * gemini_production_client_id
  * gemini_production_client_secret
  * gemini_production_fee_address
  * gemini_production_oauth_url
  * gemini_sandbox_api_url
  * gemini_sandbox_client_id
  * gemini_sandbox_client_secret
  * gemini_sandbox_fee_address
  * gemini_sandbox_oauth_url
  * goma_server_host
  * google_default_client_id
  * google_default_client_secret
  * is_brave_release_build (set to 0 in PRs)
  * rewards_grant_dev_endpoint
  * rewards_grant_prod_endpoint
  * rewards_grant_staging_endpoint
  * safebrowsing_api_endpoint
  * sardine_client_id
  * sardine_client_secret
  * updater_dev_endpoint
  * updater_prod_endpoint
  * uphold_production_api_url
  * uphold_production_client_id
  * uphold_production_client_secret
  * uphold_production_fee_address
  * uphold_production_oauth_url
  * uphold_sandbox_api_url
  * uphold_sandbox_client_id
  * uphold_sandbox_client_secret
  * uphold_sandbox_fee_address
  * uphold_sandbox_oauth_url
  * zebpay_production_api_url
  * zebpay_production_client_id
  * zebpay_production_client_secret
  * zebpay_production_oauth_url
  * zebpay_sandbox_api_url
  * zebpay_sandbox_client_id
  * zebpay_sandbox_client_secret
  * zebpay_sandbox_oauth_url
  * brave_p3a_enabled true
  * p3a_json_upload_url
  * p3a_creative_upload_url
  * p2a_json_upload_url
  * p3a_constellation_upload_url 
  * star_randomness_host
* Android
  * brave_android_developer_options_code
  * brave_android_key_password
  * brave_android_keystore_name
  * brave_android_keystore_password
  * brave_android_keystore_path
  * brave_safebrowsing_api_key
  * brave_safetynet_api_key (not set in PRs)
* macOS
  * notary_password
  * notary_user
  * sparkle_eddsa_private_key
  * sparkle_eddsa_public_key

Internal developers can find the values for these parameters in the company secrets management service (more information [here][1]).

Since each of those parameters is tied to a service that Brave has to pay to maintain, or get access to, external developers will have to supply their own values.

[1]: https://github.com/brave/devops/wiki/%60.env%60-config-for-Brave-Developers