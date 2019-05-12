---
layout: post
title: "centos gdal 2.4.1 up and running with python3.6"
description: ""
category: [centos]
tags: [proj, tiff, libgeotiff, hdf4, hdf5, netcdf, gdal, python, scl, rh-python]
---
{% include JB/setup %}


### steps

1. download source

    1. [proj-6.0.0.tar.gz](http://download.osgeo.org/proj/proj-6.0.0.tar.gz)

    1. [tiff-4.0.10.tar.gz](http://download.osgeo.org/libtiff/tiff-4.0.10.tar.gz)

    1. [libgeotiff-1.5.1.tar.gz](http://download.osgeo.org/geotiff/libgeotiff/libgeotiff-1.5.1.tar.gz)

    1. [geos-3.7.2.tar.bz2](http://download.osgeo.org/geos/geos-3.7.2.tar.bz2)

    1. [hdf-4.2.14.tar.gz](https://support.hdfgroup.org/ftp/HDF/releases/HDF4.2.14/src/hdf-4.2.14.tar.gz)

    1. [hdf5-1.10.5.tar.gz](https://support.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.10.5.tar.gz)

    1. [netcdf-c-4.7.0.tar.gz](ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-c-4.7.0.tar.gz)

    1. [gdal-2.4.1.tar.gz](http://download.osgeo.org/gdal/2.4.1/gdal-2.4.1.tar.gz)

1. install packages

    1. install [centos-release-scl]({% post_url 2017/2017-05-12-python-27-3x-on-centos-via-scls %}) and rh-python36

            $ yum install centos-release-scl
            $ yum install rh-python36
            $ scl enable rh-python36 bash

    1. install required packages to build gdal

            $ yum install ...

1. setup PATH CPATH LD_LIBRARY_PATH

    1. create export_path.sh

            $ nano /etc/profile.d/export_path.sh

            export PATH="/opt/gdal/proj-6.0.0/bin:$PATH"
            export PATH="/opt/gdal/tiff-4.0.10/bin:$PATH"
            export PATH="/opt/gdal/libgeotiff-1.5.1/bin:$PATH"
            export PATH="/opt/gdal/geos-3.7.2/bin:$PATH"
            export PATH="/opt/gdal/hdf-4.2.14/bin:$PATH"
            export PATH="/opt/gdal/hdf5-1.10.5/bin:$PATH"
            export PATH="/opt/gdal/netcdf-c-4.7.0/bin:$PATH"
            export PATH="/opt/gdal/gdal-2.4.1/bin:$PATH"
            export PATH="/root/gdal/src/gdal-2.4.1/swig/python/scripts:$PATH"
            export PATH="/usr/pgsql-10/bin/:$PATH"

            export CPATH="/opt/gdal/proj-6.0.0/include:$CPATH"
            export CPATH="/opt/gdal/tiff-4.0.10/include:$CPATH"
            export CPATH="/opt/gdal/libgeotiff-1.5.1/include:$CPATH"
            export CPATH="/opt/gdal/geos-3.7.2/include:$CPATH"
            export CPATH="/opt/gdal/hdf-4.2.14/include:$CPATH"
            export CPATH="/opt/gdal/hdf5-1.10.5/include:$CPATH"
            export CPATH="/opt/gdal/netcdf-c-4.7.0/include:$CPATH"

            export PYTHONPATH="$PYTHONPATH:/opt/gdal/gdal-2.4.1/lib64/python3.6/site-packages/"

    1. create export_libs.conf

            $ nano /etc/ld.so.conf.d/export_libs.conf
            /opt/gdal/proj-6.0.0/lib
            /opt/gdal/tiff-4.0.10/lib
            /opt/gdal/libgeotiff-1.5.1/lib
            /opt/gdal/geos-3.7.2/lib
            /opt/gdal/hdf-4.2.14/lib
            /opt/gdal/hdf5-1.10.5/lib
            /opt/gdal/netcdf-c-4.7.0/lib
            /opt/gdal/gdal-2.4.1/lib

    1. setup envirable for shell scripts

            $ nano install.0.path.sh
            echo '0.define vars'
            export proj='proj-6.0.0'
            export tiff='tiff-4.0.10'
            export libgeotiff='libgeotiff-1.5.1'
            export geos='geos-3.7.2'
            export hdf4='hdf-4.2.14'
            export hdf5='hdf5-1.10.5'
            export netcdf='netcdf-c-4.7.0'
            export gdal='gdal-2.4.1'
            export postgis='postgis-2.5.2'
            export python3_pkg='lib64/python3.6/site-packages'
            export install_dir='/opt/gdal'
            export working_dir='/root/gdal/src'
            echo ${proj}
            echo ${tiff}
            echo ${libgeotiff}
            echo ${geos}
            echo ${hdf4}
            echo ${hdf5}
            echo ${netcdf}
            echo ${gdal}
            echo ${python3_pkg}
            echo ${install_dir}
            echo ${working_dir}

            $ source install.0.path.sh

1. build source

    1. [install proj](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#proj)

            $ nano install.1.proj.sh
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${proj}

            echo '2.configure'
            cd ${working_dir}/${proj}
            ./configure --prefix=${install_dir}/${proj}

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.proj verify'
            which proj
            echo "338730 4679730" | cs2cs -v +init=epsg:26919 +to +init=epsg:4269
            # # ---- From Coordinate System ----
            # +init=epsg:26919
            # # ---- To Coordinate System ----
            # +init=epsg:4269
            # 70d57'18.075"W    42d15'11.203"N 0.000


    1. [install tiff](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#libtiff)

            $ nano install.2.tiff.sh
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${tiff}

            echo '2.configure'
            cd ${working_dir}/${tiff}
            ./configure --prefix=${install_dir}/${tiff} \
                        --exec-prefix=${install_dir}/${tiff}

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.tiff verify'
            which tiffinfo

    1. [install libgeotiff](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#geotiff)

            $ nano install.3.libgeotiff.sh
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${libgeotiff}

            echo '2.configure'
            cd ${working_dir}/${libgeotiff}
            ./configure --prefix=${install_dir}/${libgeotiff} \
                        --with-proj=${install_dir}/${proj}    \
                        --with-libtiff=${install_dir}/${tiff} \
                        --with-zlib --with-jpeg

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.libgeotiff verify'
            which listgeo


    1. [install geos](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#geos)

            $ nano install.4.geos.sh
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${geos}

            echo '2.configure'
            cd ${working_dir}/${geos}
            # https://stackoverflow.com/questions/6022384/bash-tool-to-get-nth-line-from-a-file
            sed '19213q;d' configure
            # https://unix.stackexchange.com/questions/70878/replacing-string-based-on-line-number
            # 19213                python_path=`find $i -name libpython$PYTHON_VERSION.* -print 2> /dev/null | sed "1q"`
            sed -i '19213s/$PYTHON_VERSION/3.6m/' configure
            # 19224         PYTHON_LDFLAGS="-L$python_path -lpython$PYTHON_VERSION"
            sed -i '19224s/$PYTHON_VERSION/3.6m/' configure

            ./configure --prefix=${install_dir}/${geos} \
                        --enable-python

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.geos verify'
            which geos-config
            geos-config --version


    1. [install hdf4](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#hdf4)

            $ nano install.5.hdf4.sh
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${hdf4}

            echo '2.configure'
            cd ${working_dir}/${hdf4}
            ./configure --prefix=${install_dir}/${hdf4} \
                        --with-zlib                     \
                        --with-jpeg                     \
                        --enable-shared                 \
                        --disable-netcdf                \
                        --disable-fortran

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.hdf4 verify'
            which h4cc

    1. [install hdf5](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#hdf5)

            $ nano install.6.hdf5.sh
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${hdf5}

            echo '2.configure'
            cd ${working_dir}/${hdf5}
            ./configure --prefix=${install_dir}/${hdf5} \
                        --enable-shared                 \
                        --enable-build-all              \
                        --with-zlib                     \
                        --with-pthread                  \
                        --enable-cxx

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.hdf5 verify'
            which h5cc

    1. [install netcdf](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#netcdf)

            $ nano install.7.netcdf.sh
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${netcdf}

            echo '2.configure'
            cd ${working_dir}/${netcdf}
            H4DIR="${install_dir}/${hdf4}"                   \
            H5DIR="${install_dir}/${hdf5}"                   \
            CPPFLAGS="-I${H5DIR}/include -I${H4DIR}/include" \
            LDFLAGS="-L${H5DIR}/lib -L${H4DIR}/lib"          \
            ./configure --prefix=${install_dir}/${netcdf}    \
                        --enable-netcdf-4                    \
                        --enable-shared                      \
                        --enable-utilities                   \
                        --enable-hdf4                        \
                        --enable-hdf4-file-tests             \
                        --enable-v2

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.netcdf verify'
            which nc-config

            echo '8.make check'
            make check

    1. [install gdal](http://scigeo.org/articles/howto-install-latest-geospatial-software-on-linux.html#gdal)

            # 1. build and install gdal
            echo '0.load library'
            ldconfig
            source /etc/profile.d/export_path.sh

            echo '1.cleanup install'
            rm -rf ${install_dir}/${gdal}

            echo '2.configure'
            cd ${working_dir}/${gdal}
            ./configure --prefix=${install_dir}/${gdal}             \
                        --with-jpeg=external                        \
                        --with-jpeg12                               \
                        --without-libtool                           \
                        --with-python                               \
                        --with-geos=yes                             \
                        --with-proj=${install_dir}/${proj}          \
                        --with-libtiff=${install_dir}/${tiff}       \
                        --with-geotiff=${install_dir}/${libgeotiff} \
                        --with-hdf4=${install_dir}/${hdf4}          \
                        --with-hdf5=${install_dir}/${hdf5}          \
                        --with-netcdf=${install_dir}/${netcdf}

            echo '3.make clean'
            make clean

            echo '4.make'
            make

            echo '5.make install'
            make install

            echo '6.load library'
            ldconfig

            echo '7.netcdf verify'
            which gdal-config

1. verify

    1. bins

            $ nano install.9.verify.sh
            echo '1.proj verify'
            which proj
            echo "338730 4679730" | cs2cs -v +init=epsg:26919 +to +init=epsg:4269

            echo '2.tiff verify'
            which tiffinfo

            echo '3.libgeotiff verify'
            which listgeo

            echo '4.geos verify'
            which geos-config
            geos-config --version

            echo '5.hdf5 verify'
            which listgeo

            echo '8.gdal verify'
            # check gdal version
            # (can also check built paths with --prefix --libs --dep-libs --datadir)
            gdal-config --version
            # check supported formats
            gdal-config --formats
            # see if ogr is enabled
            gdal-config --ogr-enabled
            # supported raster formats (check for tif, jpeg, png, hdf4/5, netcdf etc)
            gdalinfo --formats
            # supported vector formats (check for shapefile, kml, etc)
            ogrinfo --formats

    1. python code

            $ nano demo.py
            # coding: utf-8

            # import sys
            # oldpaths = sys.path
            # sys.path = ['/opt/gdal/gdal-2.4.1/lib64/python3.6/site-packages/']
            # sys.path.extend(oldpaths)

            from osgeo import ogr

            wkt = "POINT (1198054.34 648493.09)"
            pt = ogr.CreateGeometryFromWkt(wkt)
            bufferDistance = 500
            poly = pt.Buffer(bufferDistance)
            # ERROR 6: GEOS support not enabled.
            print("%s buffered by %d is %s" % (pt.ExportToWkt(), bufferDistance, poly.ExportToWkt())) # noqa

            # output of demo.py
            POINT (1198054.34 648493.09) buffered by 500 is POLYGON ((1198554.34 648493.09,1198553.65476738 648466.922021879,1198551.60094768 648440.825768366,1198548.1841703 648414.87276748,1198543.41380037 648389.134154591,1198537.30291314 648363.680477449,1198529.86825815 648338.581502812,1198521.13021325 648313.906025227,1198511.11272882 648289.721678462,1198499.84326209 648266.09475013,1198487.35270189 648243.09,1198473.67528397 648220.770482492,1198458.84849719 648199.197373854,1198442.91298073 648178.429804475,1198425.91241274 648158.524696821,1198407.89339059 648139.536609407,1198388.90530318 648121.517587261,1198369.00019552 648104.517019272,1198348.23262615 648088.581502812,1198326.65951751 648073.754716027,1198304.34 648060.077298108,1198281.33524987 648047.586737906,1198257.70832154 648036.317271179,1198233.52397477 648026.299786751,1198208.84849719 648017.561741852,1198183.74952255 648010.127086855,1198158.29584541 648004.016199633,1198132.55723252 647999.245829702,1198106.60423163 647995.829052316,1198080.50797812 647993.775232623,1198054.34 647993.09,1198028.17202188 647993.775232623,1198002.07576837 647995.829052316,1197976.12276748 647999.245829702,1197950.38415459 648004.016199633,1197924.93047745 648010.127086855,1197899.83150281 648017.561741852,1197875.15602523 648026.299786751,1197850.97167846 648036.317271179,1197827.34475013 648047.586737906,1197804.34 648060.077298108,1197782.02048249 648073.754716027,1197760.44737385 648088.581502812,1197739.67980448 648104.517019272,1197719.77469682 648121.517587261,1197700.78660941 648139.536609407,1197682.76758726 648158.524696821,1197665.76701927 648178.429804475,1197649.83150281 648199.197373854,1197635.00471603 648220.770482492,1197621.32729811 648243.09,1197608.83673791 648266.09475013,1197597.56727118 648289.721678462,1197587.54978675 648313.906025227,1197578.81174185 648338.581502812,1197571.37708686 648363.680477449,1197565.26619963 648389.134154591,1197560.4958297 648414.87276748,1197557.07905232 648440.825768366,1197555.02523262 648466.922021879,1197554.34 648493.09,1197555.02523262 648519.257978121,1197557.07905232 648545.354231634,1197560.4958297 648571.30723252,1197565.26619963 648597.045845409,1197571.37708686 648622.499522551,1197578.81174185 648647.598497187,1197587.54978675 648672.273974773,1197597.56727118 648696.458321538,1197608.83673791 648720.08524987,1197621.32729811 648743.09,1197635.00471603 648765.409517508,1197649.83150281 648786.982626146,1197665.76701927 648807.750195525,1197682.76758726 648827.655303179,1197700.78660941 648846.643390593,1197719.77469682 648864.662412739,1197739.67980448 648881.662980728,1197760.44737385 648897.598497187,1197782.02048249 648912.425283973,1197804.34 648926.102701892,1197827.34475013 648938.593262094,1197850.97167846 648949.862728821,1197875.15602523 648959.880213249,1197899.83150281 648968.618258148,1197924.93047745 648976.052913144,1197950.38415459 648982.163800367,1197976.12276748 648986.934170298,1198002.07576837 648990.350947684,1198028.17202188 648992.404767377,1198054.34 648993.09,1198080.50797812 648992.404767377,1198106.60423163 648990.350947684,1198132.55723252 648986.934170298,1198158.29584541 648982.163800367,1198183.74952255 648976.052913144,1198208.84849719 648968.618258148,1198233.52397477 648959.880213249,1198257.70832154 648949.862728821,1198281.33524987 648938.593262094,1198304.34 648926.102701892,1198326.65951751 648912.425283973,1198348.23262615 648897.598497187,1198369.00019552 648881.662980728,1198388.90530318 648864.662412739,1198407.89339059 648846.643390593,1198425.91241274 648827.655303179,1198442.91298073 648807.750195525,1198458.84849719 648786.982626146,1198473.67528397 648765.409517508,1198487.35270189 648743.09,1198499.84326209 648720.08524987,1198511.11272882 648696.458321538,1198521.13021325 648672.273974773,1198529.86825815 648647.598497187,1198537.30291314 648622.499522551,1198543.41380037 648597.045845409,1198548.1841703 648571.30723252,1198551.60094768 648545.354231634,1198553.65476738 648519.257978121,1198554.34 648493.09))

### fixs

1. install geos

    1. configure: error: cannot find Python library path use [sed](https://unix.stackexchange.com/questions/70878/replacing-string-based-on-line-number)

            # edit `configure` file
            $ cd ${install_dir}/${geos}
            # line 19213 replace `libpython$PYTHON_VERSION.*` with `libpython3.6m.*`
            # 19213 python_path=`find $i -name libpython$PYTHON_VERSION.* -print 2> /dev/null | sed "1q"`
            sed -i '19213s/$PYTHON_VERSION/3.6m/' configure

    1. /usr/bin/ld: cannot find -lpython3.6

            # edit `configure` file
            $ cd ${install_dir}/${geos}
            # line 19224 replace `lpython$PYTHON_VERSION` with `lpython3.6m`
            # 19224 PYTHON_LDFLAGS="-L$python_path -lpython$PYTHON_VERSION"
            sed -i '19224s/$PYTHON_VERSION/3.6m/' configure

1. demo.py

    1. ERROR 6: GEOS support not enabled.

            ERROR 6: GEOS support not enabled.
            Traceback (most recent call last):
              File "demo.py", line 14, in <module>
                print("%s buffered by %d is %s" % (pt.ExportToWkt(), bufferDistance, poly.ExportToWkt())) # noqa
            AttributeError: 'NoneType' object has no attribute 'ExportToWkt'

            # 1.build geos with python36
            $ scl enable rh-python36 bash
            $ ./install.4.geos.sh
            ...

            # 2.build gdal with python and geos
            ...

            # 3.build and install python binding
            ...
