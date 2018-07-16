## Header includes

When you are including files always use import syntax except for css and media types.

Header can be divided in the following sections
```
#First section is just imports from node_module without title
import * as React from 'react'

# Here we import all local components
// Components
import BraveScreen from './braveScreen'
import RewardsScreen from './rewardsScreen'
import ImportScreen from './importScreen'

# Here we import all constant files
//Constants
import { theme } from '../constants/theme'

# Here we import everything else (actions, reducers, api, utils, etc)
// Utils
import rewardsReducer from './rewards_reducer'
import * as newTabActions from '../actions/newTabActions'

// Assets
const background = require('../../img/welcome/welcomebg.svg')
require('../../styles/newtab.less')
require('font-awesome/css/font-awesome.css')
require('../../fonts/poppins.css')

```