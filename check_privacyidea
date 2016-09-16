#!/bin/bash
#
# 2016-09-06 Cornelius Kölbel <cornelius.koelbel@netknights.it>
#            Initial writeup
#
# This code is free software; you can redistribute it and/or
# modify it under the terms of the GNU GENERAL PUBLIC LICENSE
# License as published by the Free Software Foundation; either
# version 3 of the License, or any later version.
#
# This code is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU AFFERO GENERAL PUBLIC LICENSE for more details.
#
# You should have received a copy of the GNU General Public
# License along with this program.  
# If not, see <http://www.gnu.org/licenses/>. 
#
#
#-------------------------------------------------------------
# You might need to change this:
URL=http://localhost:5000/validate/check
# TODO: Use --verify option
CHECK_SSL=0
USER=cornelius
PIN=1234
OTP_SEED=58a0fbc8ff2c391db88ced63cf8115d1b8b947e1
REALM=themis
OTP_CMD=oathtool
# Package httpie
HTTP_CMD=http
USER_AGENT=icinga_check

# This is the token with the serial TOTP00295075
# You should use a TOTP token, since we do not save
# the HOTP counter anywhere!

OTP=`${OTP_CMD} --totp ${OTP_SEED}`
RESULT=`${HTTP_CMD} ${URL} user-agent:${USER_AGENT}  user=$USER realm=$REALM pass=${PIN}${OTP}`

echo $RESULT | grep '"status": true' - > /dev/null
SERVER_OK=$?

echo $RESULT | grep '"value": true' - > /dev/null
AUTH_OK=$?

# Return codes according to
# http://docs.icinga.org/latest/en/pluginapi.html

if [ $SERVER_OK -ne 0 ]; then
   # The server has a problem to process the request
   echo $RESULT
   exit 2
fi

if [ $AUTH_OK -ne 0 ]; then
   # The authentication failed
   echo $RESULT
   exit 1
fi

echo "Testuser $USER authenticated successfully."
exit 0
