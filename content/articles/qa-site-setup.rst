QA Site setup
#############
:date: 2017-02-04 10:58
:tags: automation, site, bash
:slug: qa-site-setup
:description: Basic script to set up QA pelican site.


.. code-block:: bash

        site_dir="~/workspace/qa-site"
        pelican_themes="~/workspace/pelican-themes"
        pelican_plugins="~/wokspace/pelican_plugins"
        apache_site="/var/www/qa.estebanm.com"


        # Install pip
        sudo apt-get install -y python3-pip

        # Update
        pip3 install -U pip setuptools

        # Install pelican
        sudo -H pip3 install pelican

        # Clone qa-site
        git clone https://github.com/emicha/qa-site.git $site_dir

        # Link pelican/output to apache's document root
        sudo ln -s $site_dir/output $apache_site/current

        # Install emicha_Flex theme
        mkdir -p $pelican_themes
        git clone https://github.com/emicha/Flex.git $pelican_themes/emicha_Flex
        sudo pelican-themes -i $pelican_themes/emicha_Flex

        # Enable tipue_search plugin
        # From scrath: http://moparx.com/2014/04/adding-search-capabilities-within-your-pelican-powered-site-using-tip$
        # Use Tipue search https://github.com/getpelican/pelican-plugins/tree/master/tipue_search
        # http://moparx.com/2014/04/adding-search-capabilities-within-your-pelican-powered-site-using-tipue-search/
        sudo pip3 install beautifulsoup4
        git clone https://github.com/getpelican/pelican-plugins $pelican_plugins
        ln -s $pelican_plugins/tipue_search $qa-site/plugins/tipue_search

        # Start pelican
        cd $site_dir && ./develop_serve.sh
        sudo systemctl restart apache2
