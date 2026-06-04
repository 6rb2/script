$(document).ready(function() {
    let backupCounter = parseInt(localStorage.getItem('busers_backup')) || 0;

    function updateCounters() {
        let visitorsElement = $('#s1');
        let totalUsersElement = $('.busers');

        let currentVisitors = parseInt(visitorsElement.attr('data-count')) || parseInt(visitorsElement.text()) || 0;
        let currentTotalUsers = parseInt(totalUsersElement.attr('data-count')) || parseInt(totalUsersElement.text()) || 0;

        backupCounter = currentTotalUsers;

        let fakeVisitors = currentVisitors + 30;
        let fakeTotalUsers = backupCounter + 30;

        if (visitorsElement.length && parseInt(visitorsElement.text()) !== fakeVisitors) {
            visitorsElement.text(fakeVisitors);
        }
        if (totalUsersElement.length && parseInt(totalUsersElement.text()) !== fakeTotalUsers) {
            totalUsersElement.text(fakeTotalUsers);
        }

        localStorage.setItem('v_data', JSON.stringify({
            's1': fakeVisitors,
            'busers': fakeTotalUsers
        }));
        localStorage.setItem('busers_backup', backupCounter);
    }

    let statsObserver = new MutationObserver(mutations => {
        statsObserver.disconnect();
        
        let visitorsElement = $('#s1');
        let totalUsersElement = $('.busers');

        if (visitorsElement.length) {
            let currentStr = visitorsElement.text().trim();
            let currentVal = parseInt(currentStr) || 0;
            if (currentVal > 0 && currentVal < 70) {
                visitorsElement.text(currentVal + 30);
            }
        }

        if (totalUsersElement.length) {
            let currentStr = totalUsersElement.text().trim();
            let currentVal = parseInt(currentStr) || 0;
            if (currentVal > 0 && currentVal < 70) {
                totalUsersElement.text(currentVal + 30);
            }
        }

        let targetS1 = document.getElementById('s1');
        if (targetS1) {
            statsObserver.observe(targetS1, {
                'childList': true,
                'characterData': true,
                'subtree': false
            });
        }
        
        totalUsersElement.each(function() {
            statsObserver.observe(this, {
                'childList': true,
                'characterData': true,
                'subtree': false
            });
        });
    });

    if ($('#s1').length) {
        statsObserver.observe(document.getElementById('s1'), {
            'childList': true,
            'characterData': true,
            'subtree': false
        });
    }

    $('.busers').each(function() {
        statsObserver.observe(this, {
            'childList': true,
            'characterData': true,
            'subtree': false
        });
    });

    window.addEventListener('message', function(event) {
        if (event.data === 'update_counters') {
            let receivedData = JSON.parse(event.data);
            $('#s1').text(receivedData.s1);
            if ($('.busers').length) {
                $('.busers').text(receivedData.busers);
            }
        }
    });

    setTimeout(updateCounters, 1000);
});
